# Hermes Agent — Self-Hosted Deployment on AWS EC2 with Dokploy & Traefik

> Deployment guide for running **Nous Research Hermes Agent v0.20.0** on an AWS EC2 instance, exposing its web dashboard securely through a custom domain using Dokploy's Traefik instance.

---

## Overview

This setup runs Hermes Agent inside Docker and exposes its web dashboard at:

**https://hermes.notme.in**

The final architecture is:

```text
                         Internet
                            │
                            ▼
                  https://hermes.notme.in
                            │
                            ▼
                    Dokploy Traefik
                         :443
                            │
                            ▼
                    Hermes Dashboard
                       :9119
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
       Dashboard APIs              Hermes Gateway
       /api/*                      :8642
                                      │
                                      ▼
                              Docker Terminal
                                      │
                                      ▼
                              Agent Workspace
```

### Infrastructure

| Component           | Value                              |
| ------------------- | ---------------------------------- |
| Cloud               | AWS EC2                            |
| OS                  | Ubuntu 24.04                       |
| Container runtime   | Docker                             |
| Deployment platform | Dokploy                            |
| Reverse proxy       | Traefik v3.6.7                     |
| Hermes image        | `nousresearch/hermes-agent:latest` |
| Hermes version      | `0.20.0`                           |
| Dashboard           | Port `9119`                        |
| Gateway/API         | Port `8642`                        |
| Docker network      | `dokploy-network`                  |
| Domain              | `hermes.notme.in`                  |
| TLS                 | Let's Encrypt via Traefik          |

---

# 1. Verify the Containers

The first step was checking the running Docker containers:

```bash
docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Networks}}'
```

Expected result:

```text
NAMES              IMAGE                                STATUS
hermes             nousresearch/hermes-agent:latest    Up
omniroute          diegosouzapw/omniroute:latest       Up
dokploy-traefik    traefik:v3.6.7                      Up
```

The important part was that Hermes and Traefik were both connected to:

```text
dokploy-network
```

This allows Traefik to communicate directly with:

```text
hermes:8642
hermes:9119
```

---

# 2. Verify Hermes Terminal Configuration

Hermes supports multiple terminal backends.

We checked:

```bash
docker exec hermes sh -c '
hermes config get terminal
'
```

The important configuration was:

```yaml
backend: docker
modal_mode: auto
degraded_mode: warn
timeout: 180
docker_image: nikolaik/python-nodejs:python3.11-nodejs20
container_cpu: 1
container_memory: 5120
container_disk: 51200
docker_network: true
persistent_shell: true
```

The critical setting is:

```yaml
backend: docker
```

This means Hermes uses a Docker-based terminal environment rather than directly executing agent terminal commands on the EC2 host.

---

# 3. Verify the Hermes Gateway

Initially, checking listening ports inside the container did not show the expected dashboard port:

```bash
docker exec hermes sh -c '
ss -lntp 2>/dev/null || true
'
```

However, Hermes logs showed:

```text
Hermes Gateway Starting...
```

and later:

```text
HERMES_DASHBOARD_READY port=9119
```

We tested the gateway directly:

```bash
docker exec hermes sh -c '
python3 - <<'"'"'PY'"'"'
import urllib.request

for url in [
    "http://127.0.0.1:8642/",
    "http://127.0.0.1:8642/health"
]:
    try:
        r = urllib.request.urlopen(url, timeout=3)
        print(url, r.status)
        print(r.read(300).decode(errors="replace"))
    except Exception as e:
        print(url, "ERROR:", e)
PY
'
```

The health endpoint returned:

```json
{
  "status": "ok",
  "platform": "hermes-agent",
  "version": "0.20.0"
}
```

So the Hermes gateway was healthy.

---

# 4. Verify Traefik → Hermes Connectivity

Before configuring the public domain, we verified that Traefik could reach Hermes.

```bash
docker exec dokploy-traefik sh -c '
wget -qO- http://hermes:8642/health || true
'
```

Result:

```json
{
  "status": "ok",
  "platform": "hermes-agent",
  "version": "0.20.0"
}
```

This confirmed:

```text
Traefik
   ↓
dokploy-network
   ↓
hermes:8642
```

was working.

---

# 5. Understand the Dokploy Traefik Configuration

Dokploy's Traefik configuration was mounted from:

```text
/etc/dokploy/traefik
```

We verified the mounts:

```bash
docker inspect dokploy-traefik \
  --format '{{range .Mounts}}{{println .Source " -> " .Destination}}{{end}}'
```

Relevant output:

```text
/etc/dokploy/traefik/traefik.yml
    -> /etc/traefik/traefik.yml

/etc/dokploy/traefik/dynamic
    -> /etc/dokploy/traefik/dynamic
```

The static Traefik configuration contained:

```yaml
providers:
  docker:
    exposedByDefault: false
    watch: true
    network: dokploy-network

  file:
    directory: /etc/dokploy/traefik/dynamic
    watch: true

entryPoints:
  web:
    address: :80

  websecure:
    address: :443
```

This meant the easiest and safest way to add the Hermes domain was through a dynamic configuration file.

---

# 6. Initial Hermes Routing

The first configuration exposed the Hermes gateway:

```yaml
http:
  routers:
    hermes-router:
      rule: "Host(`hermes.notme.in`)"
      entryPoints:
        - websecure
      service: hermes-service
      tls:
        certResolver: letsencrypt

  services:
    hermes-service:
      loadBalancer:
        servers:
          - url: "http://hermes:8642"
        passHostHeader: true
```

Traefik successfully discovered:

```text
hermes-service
hermes:8642
UP
```

However:

```bash
curl -I https://hermes.notme.in/
```

returned:

```text
HTTP/2 404
```

This was expected because the gateway on `8642` is an API/gateway service, not the dashboard UI.

---

# 7. Discover the Hermes Dashboard

Hermes provides a separate web dashboard.

We checked:

```bash
docker exec hermes sh -c '
hermes dashboard --help
'
```

Important output:

```text
Launch the Hermes Agent web dashboard
```

The default dashboard port is:

```text
9119
```

The dashboard requires authentication when exposed on a non-loopback interface.

Hermes explicitly rejected:

```bash
hermes dashboard --host 0.0.0.0 --port 9119
```

with:

```text
Refusing to bind dashboard to 0.0.0.0
```

because no public authentication provider had been registered.

---

# 8. Configure Dashboard Authentication

Hermes already had basic authentication configured.

We checked:

```bash
docker exec hermes sh -c '
hermes config get dashboard
'
```

The configuration contained:

```yaml
dashboard:
  basic_auth:
    username: admin
    password_hash: ...
```

The username was:

```text
admin
```

The password hash must **not** be treated as the password.

A value such as:

```text
scrypt$16384$8$1$...
```

is a one-way password hash.

If the original password is forgotten, it should be reset rather than attempting to reverse the hash.

---

# 9. Start the Dashboard

Hermes dashboard was started on:

```text
9119
```

The dashboard became available internally at:

```text
http://127.0.0.1:9119
```

We verified:

```bash
docker exec hermes sh -c '
python3 - <<'"'"'PY'"'"'
import urllib.request

for path in ["/", "/login", "/auth/password-login", "/health"]:
    try:
        r = urllib.request.urlopen(
            "http://127.0.0.1:9119" + path,
            timeout=3
        )
        print(path, r.status)
    except Exception as e:
        print(path, e)
PY
'
```

Result:

```text
/                    200
/login               200
/auth/password-login 200
/health              200
```

---

# 10. Route the Dashboard Through Traefik

The final configuration was simplified significantly.

Instead of manually routing individual paths such as:

```text
/
/login
/auth
/assets
/dashboard
/api
```

we route the entire hostname to the dashboard.

Create:

```text
/etc/dokploy/traefik/dynamic/hermes.yml
```

with:

```yaml
http:
  routers:

    hermes-dashboard:
      rule: "Host(`hermes.notme.in`)"
      entryPoints:
        - websecure
      service: hermes-dashboard
      priority: 100
      tls:
        certResolver: letsencrypt

  services:

    hermes-dashboard:
      loadBalancer:
        servers:
          - url: "http://hermes:9119"
        passHostHeader: true
```

This is the final routing model:

```text
hermes.notme.in
       │
       ▼
    Traefik
       │
       ▼
 hermes:9119
       │
       ├── /
       ├── /login
       ├── /auth/*
       ├── /api/*
       ├── /sessions
       ├── /chat
       ├── /files
       ├── /models
       ├── /skills
       └── ...
```

This was preferable to maintaining a manually enumerated list of dashboard routes.

---

# 11. Verify the Traefik Router

Check:

```bash
docker exec dokploy-traefik sh -c '
wget -qO- http://127.0.0.1:8080/api/http/routers 2>/dev/null
' | python3 -m json.tool | grep -A15 -B3 -i "hermes"
```

Expected:

```text
service: hermes-dashboard
rule: Host(`hermes.notme.in`)
priority: 100
entryPoints:
  - websecure
status: enabled
```

---

# 12. Verify the Dashboard API

The most important verification was:

```bash
curl -i https://hermes.notme.in/api/status
```

Expected:

```text
HTTP/2 200
```

The response confirmed:

```json
{
  "version": "0.20.0",
  "gateway_running": true,
  "gateway_state": "running",
  "auth_required": true,
  "auth_providers": [
    "basic"
  ],
  "auth_flows": [
    "cookie"
  ]
}
```

This confirmed that:

* Hermes is running
* Gateway is running
* Dashboard is running
* Authentication is enabled
* Basic authentication is registered
* Cookie-based dashboard authentication is active

---

# 13. Verify Authentication Providers

Run:

```bash
curl -i https://hermes.notme.in/api/auth/providers
```

Expected:

```json
{
  "providers": [
    {
      "name": "basic",
      "display_name": "Username & Password",
      "supports_password": true
    }
  ]
}
```

This confirms the dashboard supports:

```text
Username & Password
```

---

# 14. Important: `/api/auth/ws-ticket`

One confusing part during debugging was:

```text
/api/auth/ws-ticket → 401
```

when tested directly without authentication.

This is expected.

The endpoint generates an authenticated WebSocket/PTY ticket and therefore should not return success to an unauthenticated request.

The correct flow is:

```text
User
 ↓
Login
 ↓
Hermes creates authenticated session cookie
 ↓
Browser calls /api/auth/ws-ticket
 ↓
Hermes validates cookie
 ↓
PTY/WebSocket ticket issued
 ↓
Chat terminal connects
```

Therefore, testing:

```bash
curl https://hermes.notme.in/api/auth/ws-ticket
```

without the login cookie is **not a valid test of the authenticated dashboard**.

---

# 15. Final Verification

The final Hermes CLI confirmed the agent itself was operational.

Hermes displayed:

```text
Hermes Agent v0.20.0
```

and:

```text
31 tools
73 skills
```

A test prompt:

```text
hi
```

was successfully processed.

The session became:

```text
ready
```

with a valid session ID.

This verified the complete agent execution stack.

---

# 16. Final Architecture

```text
                         ┌──────────────────────┐
                         │       Internet       │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         hermes.notme.in:443
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │       Traefik        │
                         │      Dokploy         │
                         └──────────┬───────────┘
                                    │
                         dokploy-network
                                    │
                                    ▼
                    ┌─────────────────────────────┐
                    │      Hermes Container       │
                    │                             │
                    │  Dashboard :9119            │
                    │       │                     │
                    │       ├── Authentication   │
                    │       ├── Dashboard API    │
                    │       ├── Sessions         │
                    │       ├── Chat             │
                    │       └── WebSocket/PTTY   │
                    │                             │
                    │  Gateway :8642             │
                    │       │                     │
                    │       ▼                     │
                    │  Docker Terminal Backend   │
                    └─────────────┬───────────────┘
                                  │
                                  ▼
                         Agent Docker Environment
```

---

# 17. Useful Commands

### Check Hermes

```bash
docker ps --filter name=hermes
```

### Hermes logs

```bash
docker logs --tail 200 hermes
```

### Check dashboard

```bash
docker exec hermes sh -c '
ss -lntp 2>/dev/null | grep 9119 || true
'
```

### Check gateway

```bash
curl https://hermes.notme.in/health
```

### Check dashboard status

```bash
curl https://hermes.notme.in/api/status
```

### Check authentication providers

```bash
curl https://hermes.notme.in/api/auth/providers
```

### Check Traefik routers

```bash
docker exec dokploy-traefik sh -c \
'wget -qO- http://127.0.0.1:8080/api/http/routers'
```

### Check Traefik services

```bash
docker exec dokploy-traefik sh -c \
'wget -qO- http://127.0.0.1:8080/api/http/services'
```

### Test Hermes from Traefik

```bash
docker exec dokploy-traefik sh -c \
'wget -qO- http://hermes:9119/api/status'
```

---

# 18. Troubleshooting Lessons

## `https://hermes.notme.in/` returns 404

Check whether Traefik is pointing to:

```text
hermes:8642
```

The gateway is not the dashboard.

The dashboard is:

```text
hermes:9119
```

---

## `/api/status` returns 404

Check the Traefik router.

If `/api/status` is being sent to:

```text
hermes:8642
```

it will fail.

The entire Hermes hostname should ultimately route to:

```text
hermes:9119
```

for the dashboard.

---

## `/api/auth/ws-ticket` returns 401

If tested without authentication, this is expected.

Log into the dashboard first and test from the browser.

---

## Dashboard refuses `0.0.0.0`

Hermes intentionally requires an authentication provider for public dashboard binds.

Configure:

```yaml
dashboard:
  basic_auth:
    username: admin
    password_hash: ...
```

or use the supported OAuth/dashboard authentication mechanism.

---

## Traefik cannot reach Hermes

Check that both containers are connected to:

```text
dokploy-network
```

Then:

```bash
docker exec dokploy-traefik sh -c \
'wget -qO- http://hermes:9119/'
```

A healthy dashboard should return:

```text
HTTP/1.1 302 Found
location: /login?next=%2F
```

---

# 19. Security Notes

Do **not** expose the Hermes gateway directly to the public internet unless there is a specific reason to do so.

The preferred public entry point is:

```text
HTTPS → Traefik → authenticated Hermes Dashboard
```

The dashboard should retain authentication:

```yaml
auth_required: true
```

Never commit the actual Hermes password or password hash to GitHub.

Never commit:

```text
config.yaml
.env
API keys
OAuth credentials
AWS credentials
private keys
ACME credentials
session cookies
```

Add sensitive files to `.gitignore`.

Example:

```gitignore
.env
.env.*
config.yaml
*.pem
*.key
acme.json
secrets/
```

---

# 20. Final Status

The deployment is operational.

```text
┌─────────────────────────────────────────────┐
│              HERMES DEPLOYMENT              │
├─────────────────────────────────────────────┤
│                                             │
│  Docker                         ✅           │
│  Hermes Agent                   ✅           │
│  Hermes Gateway :8642           ✅           │
│  Hermes Dashboard :9119        ✅           │
│  Docker terminal backend        ✅           │
│  Dokploy                        ✅           │
│  Traefik                        ✅           │
│  TLS / HTTPS                    ✅           │
│  DNS                            ✅           │
│  Dashboard authentication       ✅           │
│  Dashboard API                  ✅           │
│  Agent execution                ✅           │
│                                             │
│  https://hermes.notme.in        ✅           │
│                                             │
└─────────────────────────────────────────────┘
```

The key lesson from the deployment was:

> **Hermes Gateway (`8642`) and Hermes Dashboard (`9119`) are separate services. The public Hermes hostname should route to the dashboard on `9119`, while the dashboard internally communicates with the Hermes gateway.**

---
