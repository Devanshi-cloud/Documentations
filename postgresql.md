# PostgreSQL Setup & Debugging (macOS · Homebrew)

## 1. Initial problem

Command:

```bash
psql --version
```

Error:

```bash
zsh: command not found: psql
```

Interpretation: PostgreSQL CLI (`psql`) was not installed or not on my `PATH` yet. [dev](https://dev.to/uponthesky/postgresql-installing-postgresql-through-homebrew-on-macos-388h)

***

## 2. Installing PostgreSQL

Command:

```bash
brew install postgresql
```

What happened:

- Homebrew installed PostgreSQL 18 on my Mac. [dev](https://dev.to/celestialchips/setting-up-postgres-via-homebrew-2lbe)
- The default database cluster was initialized at: `/opt/homebrew/var/postgresql@18`. [dev](https://dev.to/celestialchips/setting-up-postgres-via-homebrew-2lbe)

***

## 3. Starting the PostgreSQL service

Command:

```bash
brew services start postgresql
```

Result:

- PostgreSQL server started successfully as a background service. [geeksforgeeks](https://www.geeksforgeeks.org/postgresql/how-to-install-postgresql-on-a-mac-with-homebrew/)

Verification:

```bash
psql --version
# psql (PostgreSQL) 18.3 (Homebrew)
```

***

## 4. First mistake: running SQL in the shell

What I tried in zsh:

```bash
CREATE DATABASE myapp;
```

Error:

```bash
zsh: command not found: CREATE
```

What I learned:

- `CREATE DATABASE` is a SQL command.  
- SQL commands must run inside the PostgreSQL shell (`psql`), not the regular terminal. [geeksforgeeks](https://www.geeksforgeeks.org/postgresql/how-to-install-postgresql-on-a-mac-with-homebrew/)

***

## 5. Entering the PostgreSQL shell

Command:

```bash
psql postgres
```

Prompt changed to:

```text
postgres=#
```

Meaning: I was now inside the `psql` shell, connected to the `postgres` database.

***

## 6. Creating my application database

Inside `psql`:

```sql
CREATE DATABASE omaxe_treasury;
```

Result:

```text
CREATE DATABASE
```

My database `omaxe_treasury` was successfully created.

***

## 7. Second issue: missing `postgres` role

What I tried (still inside `psql`):

```sql
ALTER USER postgres WITH PASSWORD 'password';
```

Error:

```text
ERROR:  role "postgres" does not exist
```

Root cause (on macOS with Homebrew PostgreSQL):

- Homebrew creates a default PostgreSQL role with my macOS username, not `postgres`. [stackoverflow](https://stackoverflow.com/questions/23769579/default-password-for-my-user-in-postgresql)
- In my case, the default role is `devanshi`; the `postgres` role was never created.

***

## 8. Confirming my actual database user

Inside `psql`:

```sql
SELECT current_user;
```

Output:

```text
 current_user
-------------
 devanshi
```

So the role I should use is `devanshi`, not `postgres`. [stackoverflow](https://stackoverflow.com/questions/23769579/default-password-for-my-user-in-postgresql)

***

## 9. Correct way to connect

Option A (recommended, no password):

```env
DATABASE_URL="postgresql://devanshi@localhost:5432/omaxe_treasury"
```

Option B (with password set on my user):

Inside `psql`:

```sql
ALTER USER devanshi WITH PASSWORD 'password';
```

Then in `.env`:

```env
DATABASE_URL="postgresql://devanshi:password@localhost:5432/omaxe_treasury"
```

This URL is what my Node.js app will use via `pg`. [node-postgres](https://node-postgres.com/features/connecting)

***

## 10. Third mistake: treating env vars as SQL

What I mistakenly did inside `psql`:

```sql
DATABASE_URL="postgresql://devanshi@localhost:5432/omaxe_treasury";
```

Issue:

- This is not valid SQL.  
- Environment variables like `DATABASE_URL` belong in the app layer (`.env`, Node.js), not inside the PostgreSQL shell. [node-postgres](https://node-postgres.com/features/connecting)

***

## 11. Final connection test from the terminal

Command:

```bash
psql -U devanshi -d omaxe_treasury
```

Prompt:

```text
omaxe_treasury=#
```

Meaning:

- The database `omaxe_treasury` exists.  
- The `devanshi` role works.  
- Local connection is successful.

***

## 12. Optional sanity check: test table

Inside `psql`:

```sql
CREATE TABLE test (
  id SERIAL PRIMARY KEY,
  name TEXT
);

INSERT INTO test (name) VALUES ('Devanshi');

SELECT * FROM test;
```

This confirmed that I could create tables, insert rows, and query data.

***

## 13. Node.js integration (pg)

Install the dependency:

```bash
npm install pg
```

Sample connection code:

```js
import pkg from 'pg';
const { Pool } = pkg;

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

const res = await pool.query('SELECT NOW()');
console.log(res.rows);
```

This uses the same `DATABASE_URL` I configured earlier and tests a simple query through a connection pool. [stackoverflow](https://stackoverflow.com/questions/44643652/using-postgres-with-nodejs-for-connection-pool)

***

## 14. Final status

| Item                     | Status |
|--------------------------|--------|
| PostgreSQL installed     | ✅     |
| Service running          | ✅     |
| Database created         | ✅     |
| Correct user identified  | ✅     |
| Manual psql connection   | ✅     |
| Node.js integration base | ✅     |

***

## 15. What I learned

- SQL commands like `CREATE DATABASE` must run inside `psql`, not in the regular shell.  
- With Homebrew on macOS, PostgreSQL’s default role matches my macOS username (in my case, `devanshi`), not `postgres`. [github](https://github.com/orgs/Homebrew/discussions/4777)
- Environment variables such as `DATABASE_URL` live in the application layer (`.env`, Node.js), not inside the database shell. [node-postgres](https://node-postgres.com/features/connecting)
- Manually testing the database connection with `psql` before wiring up code makes debugging the backend much easier. [geeksforgeeks](https://www.geeksforgeeks.org/postgresql/how-to-install-postgresql-on-a-mac-with-homebrew/)
