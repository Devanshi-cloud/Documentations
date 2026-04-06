# Lead Scraping Approaches & Lessons Learned

## 1. Core Working Method: Google SERP + AI Structuring
**Summary**  
Leverage Google search results (bypassing direct LinkedIn scraping) and use AI to parse unstructured data into clean leads. This is scalable, low-risk, and cost-effective.

**Steps**  
- Craft precise queries: `site:linkedin.com "Job Title" "Industry" "Location"` (e.g., `site:linkedin.com "CTO" mining "Australia"`).  
- Query via SERP APIs like SerpAPI, HasData, or DataForSEO.  
- Capture: title, link, snippet.  
- Scale by looping: job titles × locations × industries.  
- Export raw data to JSON/CSV.  
- Pipe to LLM (e.g., Claude via Langchain or Gemini API): extract name, title, company, location.  

**Optional Enrichment**  
- Reverse-search profiles for emails or GitHub/Twitter.  

**Outcome**  
- 1,000+ structured leads/day at ~$0.01/lead.  
- Compliance-friendly (public data only).

## 2. Email Enrichment: Secondary Google Queries
**Summary**  
Boost lead quality by hunting emails post-profiling.

**Steps**  
- Use extracted fields: `"First Last" "Company" email OR contact`.  
- SERP API query → regex/LLM for email patterns (e.g., `john@company.com`).  
- Validate with Hunter.io or NeverBounce API.  

**Limitations & Mitigations**  
- ~30-50% hit rate; counter with multi-source queries.  
- False positives; add domain verification.  

**Outcome**  
- Turns profiles into actionable contacts.

## 3. Company-First Scraping + Role Extraction
**Summary**  
Build a company database, then pull decision-makers for precise B2B targeting.

**Steps**  
- From initial leads, dedupe companies.  
- Query: `"Company" CEO site:linkedin.com`, `"Company" headquarters`.  
- LLM structures: CEO name, LinkedIn page, industry, HQ.  
- Expand to VP Sales/Engineering via role loops.  

**Outcome**  
- Rich B2B datasets for targeted outreach.

## 4. Advanced Approach: LinkedIn Sales Navigator + Proxies
**Summary**  
For higher precision, use Sales Navigator's filters with rotating proxies to mimic human behavior (avoid direct scraping bans).

**Steps**  
- Auth via Puppeteer/Playwright in Docker.  
- Filters: title, company size, geography.  
- Export to CSV → AI structuring.  
- Rotate proxies (BrightData/ProxyMesh) + headless delays.  

**Pro Tip**  
Integrate with Vercel serverless for burst scaling.

## 5. Enrichment Boost: Reverse WHOIS & GitHub Mining
**Summary**  
Layer domain intel and OSS footprints for tech leads.

**Steps**  
- WHOIS query company domain → founder emails.  
- `"Company" GitHub` → extract contributors' profiles/emails.  

**Outcome**  
- Uncovers hidden tech decision-makers.

## Failed Attempts & Pitfalls
### Google Custom Search API + n8n
**What Failed**  
- 10-result cap, no rich snippets, pagination hell, domain locks.  
- n8n workflows bloated and brittle at 10k+ queries.  

**Lesson**  
Stick to full SERP APIs for scale; Custom Search suits tiny, controlled searches only.

### Direct LinkedIn Scraping (Puppeteer)
**Issues**  
- IP bans after 100 profiles.  
- CAPTCHA walls, UI changes break selectors.  

**Lesson**  
Google SERP is 10x safer and broader.

## Key Lessons Learned
- **AI is Non-Negotiable**: Messy SERPs → 90% clean data via prompt engineering (e.g., "Extract JSON: {name, title, company}").  
- **Scale Smart**: Batch queries, rate-limit (1/sec), use async (Node.js/Golang).  
- **Legal/Ethical Guardrails**: Public data only; GDPR-compliant (no PII storage without consent); disclose scraping in outreach.  
- **Cost Optimization**: SerpAPI free tier → $50/mo for 10k queries.  
- **Tech Stack for Devs**: SerpAPI + Langchain (LLM) + MongoDB (storage) + Vercel (deploy). Dedupe with fuzzy matching (e.g., Levenshtein distance).  
- **Pitfall**: Over-reliance on emails—prioritize LinkedIn URLs for warm intros.

## Final Takeaways
- **Winner**: SERP API + AI = scalable, low-cost leads.  
- Avoid: Direct platform scrapes, restrictive APIs.  
- Google > LinkedIn for discovery.

## Next Improvements
- **Validation Pipeline**: Dedupe (RecordLinkage lib), email verify (ZeroBounce).  
- **Automation**: Airflow/n8n DAGs on AWS/GCP.  
- **Storage/CRM**: Pinecone for vector search → HubSpot sync.  
- **Monitoring**: Track ban rates, enrichment yield.
