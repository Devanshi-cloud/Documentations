# Guide to Diagnose and Resolve the “550 5.7.24 SPF Permanent Error”  

## 1. What the SPF Failure Means  

The bounce tells the receiving server (IIT B’s mail system) that the SPF check for the sender domain **hypotenactanalytics.com** failed with a *Permanent Error* because an **include** mechanism points to a sub-domain that does not publish a valid SPF record:

```
No valid SPF record for included domain: dc-aa8e722993._spfm.hypotenuseanalytics.com:
include:dc-aa8e722993._spfm.hypotenuseanalytics.com
```

In SPF terminology, the `include:` mechanism tells the receiver to fetch the SPF record of the named domain and treat it as part of the sender’s policy. If that domain has **no SPF TXT record** or the record is syntactically invalid, the evaluation ends with **PermError** (permanent error) - the receiver must reject the message (RFC 7208 §4.6.7) - exactly what the 550 5.7.24 code indicates [1].

The most common cause is a missing or malformed TXT record for the included sub-domain. A missing record triggers a PermError, and many mail receivers (including Google’s) will reject the message outright (source 1).

## 2. Inspect the Current DNS SPF Configuration  

### 2.1 Records that should exist  

1. **Root domain** `hypotenuseanalytics.com` - a single TXT record that starts with `v=spf1`.  
2. **Sub-domain** `dc-aa8e722993._spfm.hypotenuseanalytics.com` - also a TXT record beginning with `v=spf1` (the record that the include points to).

Only one SPF TXT record per name is allowed; multiple records cause a PermError (source 4).  

### 2.2 Using command-line tools  

- **dig** (Linux/macOS)  

```bash
dig +short TXT hypotenuseanalytics.com
dig +short TXT dc-aa8e722993._spfm.hypotenuseanalytics.com
```  

The output should show a string that begins with `v=spf1`. If nothing is returned, the record is missing.  

- **nslookup** (Windows)  

```cmd
nslookup -type=TXT hypotenuseanalytics.com
nslookup -type=TXT dc-aa8e722993._spfm.hypotenuseanalytics.com
```  

### 2.3 Online SPF validators  

- **MXToolbox SPF Record Lookup** - paste the domain name; the tool reports syntax errors, missing records, and the total number of DNS lookups (source 3, source 2 of the second set).  
- **Kitterman SPF Validator** - similar functionality, useful for confirming that each mechanism (including any `include:`) resolves correctly.

Both tools will flag a *PermError* if the included domain lacks a valid SPF record.  

## 3. Checklist for Correcting the SPF Record  

1. **Confirm a single SPF TXT record for the root domain**  

   Example (replace with your actual sending IPs or includes):  

   ```
   v=spf1 ip4:203.0.113.0/24 include:_spf.google.com ~all
   ```  

- `ip4`/`ip6` entries list the IP ranges you actually use.  
   - `include:_spf.google.com` is the official Google Workspace SPF include (Google’s documentation).

2. **Create a valid SPF record for the included sub-domain**  

The sub-domain must publish its own SPF TXT record. If you do not need a separate policy, you can simply point the sub-domain to the same policy as the root:

   ```
   v=spf1 include:_spf.google.com ~all
   ```  

   Publish this as a TXT record for `dc-aa8e722993._spfm.hypotenuseanalytics.com`.  

3. **Avoid exceeding the 10-lookup limit**  

Every `include`, `a`, `mx`, `exists`, and `redirect` triggers a DNS query. Nested includes (e.g., Google’s SPF includes several others) count toward the limit. Use a tool like MXToolbox or AutoSPF to count lookups; keep the total ≤ 10 (source 3, source 3 of second set). If you approach the limit, consider *flattening* the record (replace includes with explicit IP ranges) or using a third-party SPF-flattening service.

4. **Validate syntax**  

- No extra spaces before or after the record.  
   - No stray commas, dashes, or uppercase letters that could break parsing (source 2).  
   - The record must be a single string without line breaks.

5. **Test before publishing**  

- Use `dig` to fetch the new TXT record.  
   - Run the MXToolbox or Kitterman validator again; ensure the result is **Pass** for a test IP that you know is authorized.

6. **Publish the record**  

Add the TXT record via your DNS provider’s control panel. Propagation usually takes a few minutes to a couple of hours depending on TTL.

## 4. Supplemental Authentication (DKIM & DMARC)  

Even with a correct SPF, adding DKIM and DMARC greatly improves deliverability:  

- **DKIM** signs each outgoing message with a private key; the public key is published as a TXT record (`default._domainkey.hypotenuseanalytics.com`). Most ESPs (Google Workspace, SendGrid, etc.) provide the selector and key.

- **DMARC** tells receivers how to treat messages that fail SPF or DKIM. A basic policy for testing:  

  ```
  v=DMARC1; p=none; rua=mailto:postmaster@hypotenuseanalytics.com
  ```  

  Once you are confident SPF/DKIM pass, raise `p=` to `quarantine` or `reject` to protect your domain (source 2).  

Both mechanisms are independent of SPF but give the receiver more confidence that the message really originates from you.

## 5. Recipient-Side Considerations  

The bounce references **ashish.dutta@iitb.ac.in**, yet the verified faculty directory lists **adutta@iitk.ac.in** as the official address for Professor Ashish Dutta (source 9). It is possible that the IIT B address is outdated or a typo.

**Steps to verify:**  

1. Search the IIT Bombay faculty directory for “Ashish Dutta”.  
2. If no matching entry appears, contact the department (e.g., Mechanical Engineering) via the generic departmental email or phone to confirm the correct address.  
3. Use the confirmed address in future correspondence.

If the address is indeed invalid, the remote server may reject the message even after SPF is fixed.  

## 6. Additional Troubleshooting if SPF Fixes Fail  

1. **Ask the recipient to whitelist your sending IP** - Provide the exact IP (e.g., Google’s outbound mail IPs) and request a temporary allow-list entry on `fmlr2.iitb.ac.in`.

2. **Send from an alternate domain** - If your own domain’s SPF remains problematic, consider using a well-known domain (e.g., a Google Workspace domain you control) that already has a correct SPF record.

3. **Use a reputable email-delivery service** - Services such as SendGrid, Mailgun, or Amazon SES provide their own SPF includes and often handle flattening automatically.

4. **Contact IIT B’s mail administrator** - Include the full bounce message, the SPF record you now publish, and ask whether any additional filters (e.g., custom blocklists) are applied.

5. **Check for DNS propagation** - After updating SPF, verify that the new TXT record is visible from multiple public DNS resolvers (e.g., Google DNS `8.8.8.8`, Cloudflare `1.1.1.1`).

## 7. Summary of Action Items  

- Verify that **both** `hypotenuseanalytics.com` **and** `dc-aa8e722993._spfm.hypotenuseanalytics.com` have a single, correctly-formatted SPF TXT record.  
- Ensure the included sub-domain’s record is not missing; if you do not need a separate policy, create a minimal SPF record for it that references the same includes/IPs as the root.  
- Run the record through an online validator (MXToolbox, Kitterman) to confirm no syntax errors and that total DNS lookups ≤ 10.  
 - Publish the corrected records, wait for propagation, then re-send a test email to the IIT B address.  
- If the recipient address is outdated, locate the correct faculty email via IIT B’s directory or departmental contact.  
- Implement DKIM and a basic DMARC policy to strengthen authentication and reduce future rejections.  
- If problems persist, pursue whitelisting, alternate sending domains, or direct coordination with the IIT B mail admin.

Following these steps should eliminate the “550 5.7.24 SPF Permanent Error” and restore successful delivery to the intended recipient.

---

### Sources
- [1] https://www.rfc-editor.org/info/rfc7208
