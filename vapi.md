# How to Obtain Free (or Trial) Indian Phone Numbers for Use with Vapi  

---

## 1. What Vapi Provides - Free Numbers Are US-Only  

Vapi’s “Free Telephony” feature lets you create up to 10 free phone numbers, but **they are limited to U.S. area codes**. International numbers, including Indian numbers, are **not free** and must be imported from another provider [1][2].

### Import-by-Own (BYO) Workflow  

Vapi supports a BYO-phone-number import that uses your Twilio (or other provider) credentials to verify and configure the number for Vapi services [2]. This is the only way to use an Indian number on Vapi at the moment.

---

## 2. Third-Party Providers That Can Supply an Indian Number  

| Provider | Free / Trial Availability | How to Get an Indian Number | Key Limits for Free Tier |
|----------|--------------------------|-----------------------------|--------------------------|
| **Twilio** | $15 trial credit, one trial number (US only). Indian numbers can be purchased but cost ≈ $1.15 / month; the trial account can buy a single Indian number after verification [3][4][5] | Sign up → verify → buy an Indian local or mobile number from the console. | 1 phone number, 4 concurrent calls, trial message played on each call, outbound calls only to verified numbers. |
| **Plivo** | Trial credits (no permanent free numbers). Indian numbers can be rented on Pay-As-You-Go for ₹250 / month (≈ $3) [6][7] | Sign up → add trial credit → request an Indian local number via console or API. | Trial credits expire; number rental is paid after trial. |
| **Vonage (formerly Nexmo)** | Free trial account provides a test number that **cannot be used for live voice calls**; you must purchase a real Indian number [8][9] | Sign up → upgrade to a paid plan → buy an Indian local number. | No free voice-capable Indian number; trial number only for API testing. |
| **SIP-Trunk Providers (e.g., Global Call Forwarding, 3CX-certified, Pulse, Tata Communications)** | Usually a 15-day free trial of the SIP service; you must rent an Indian DID (local or toll-free) as part of the trunk [10][11] | Sign up for trial → obtain SIP credentials → request an Indian DID from the provider’s portal. | Trial period only; DID rental incurs a monthly fee after trial. |

> **Bottom line:** There is **no permanently free Indian number** from any major provider. The cheapest path is to use a **trial or low-cost rental** (Twilio, Plivo, or a SIP-trunk trial) and then import that number into Vapi.

---

## 3. Step-by-Step: Importing an Indian Number into Vapi  

1. **Acquire the Indian number** from one of the providers above (e.g., buy a +91 XXXXXXXXXX local number on Twilio).  
2. **Gather credentials** required by Vapi:  
   * Twilio → Account SID and Auth Token (found in the Twilio Console).  
   * For SIP-trunk providers → SIP username, password, and SIP server address.  
3. **Create a “BYO Phone Number” in Vapi**:  
   * In the Vapi Dashboard go to **Phone Numbers → Import** (or call the `/phone-numbers/import` endpoint).  
   * Choose **Twilio** (or “byo-phone-number” provider) and paste the credential ID [12].  
   * Provide the E.164 formatted Indian number (+91…) and optionally assign an assistant ID for inbound routing.  
4. **Verify the number** - Vapi will use the supplied credentials to confirm ownership with the upstream provider.  
5. **Attach the number to an assistant** (inbound) or use its `phoneNumberId` in the `/call` API to make outbound calls [13].

---

## 4. Configuring Outbound Calls from Vapi  

| Method | Required Fields | Example (cURL) |
|--------|----------------|----------------|
| **Dashboard** | Select the imported Indian number, choose an assistant, click **Place Call**. | - |
| **API** | `assistantId`, `phoneNumberId`, `customers` (array of target numbers). | `curl -X POST  -H "Authorization: Bearer <token>" -d '{"assistantId":"<aid>","phoneNumberId":"<pid>","customers":[{"number":"+919876543210"}]}'` [13] |

Vapi’s **Trusted Calling** page notes that free numbers have a **daily outbound-call cap**, while imported numbers have **no such limit** [13].

---

## 5. Limitations of Free / Trial Options  

| Aspect | Free US Numbers (Vapi) | Imported Indian Number (Trial) |
|--------|------------------------|--------------------------------|
| **Outbound volume** | Limited daily caps (unspecified, but “free numbers have limited number of outbound calls per day”) [13]. | Determined by provider (Twilio trial: 4 concurrent calls; Plivo: depends on credit; SIP-trunk: as per trial agreement). |
| **Inbound** | Fully supported for US numbers. | Supported once the number is imported; inbound routing works the same as any Vapi number. |
| **Cost** | $0 (US only). | Small monthly rental (≈ $1-$3) after trial; possible trial credits. |
| **Caller-ID (CNAM) & STIR/SHAKEN** | Vapi Trust Hub can register CNAM and attestation for US numbers [13]. | For Indian numbers, CNAM is **not available** (Plivo lists “CNAM not yet available” for India [7], and Indian carriers have limited CNAM support [7]). STIR/SHAKEN is not yet mandated in India [14]. |
| **Regulatory compliance** | Must follow US TCPA (Vapi provides a consent guide) [15]. | Must obey Indian TRAI rules (see below). |

---

## 6. Indian Regulatory & Compliance Considerations  

1. **TRAI Outbound Calling Rules** - Commercial/telemarketing calls may be placed only **between 09:00 and 21:00 IST**; calls outside this window breach the Telecom Commercial Communications Customer Preference Regulations (TCCCPR) .  
2. **Do-Not-Disturb (DND) & DLT** - Outbound callers must scrub numbers against the **Distributed Ledger Technology (DLT) registry** and must have **explicit consent** for marketing calls. Failure can lead to caps, warnings, or blacklisting [16][17].  
3. **140-Series Numbers** - Bulk telemarketing is required to use numbers in the **140 prefix range**; using a regular mobile number for mass outbound calls may be considered non-compliant [16].  
4. **Consent** - Prior Express Consent (PEC) for non-marketing and Prior Express Written Consent (PEWC) for marketing calls are mandatory; Vapi’s TCPA guide outlines similar consent requirements for the US, which you should replicate for India [15].  
5. **Caller-ID (CNAM) & STIR/SHAKEN** - Indian carriers currently **do not provide CNAM** for virtual numbers, and STIR/SHAKEN is not yet enforced in India, so the caller name may not appear on the recipient’s handset [7][14].

**Practical tip:** When using an Indian number for outbound AI-driven calls, configure your Vapi assistant to prepend a consent script, limit call windows to 09-21 IST, and maintain a DND-scrubbed list.

---

## 7. Comparison Summary  

| Provider | Free Tier? | Approx. Cost After Trial | Ability to Import into Vapi | CNAM / Caller-ID | Typical Trial Limits |
|----------|------------|--------------------------|-----------------------------|------------------|----------------------|
| Vapi (US free) | Yes (US only) | $0 | N/A (US numbers only) | Available via Trust Hub (US) | Daily outbound cap |
| Twilio | $15 credit (one trial number) | $1.15 / mo for Indian number | Yes (via Twilio credentials) | No CNAM for Indian numbers | 4 concurrent calls, trial message |
| Plivo | Credit trial (no permanent free number) | ₹250 / mo (~$3) | Yes (via Plivo credentials) | Not supported for India | Credit expires; paid after |
| Vonage/Nexmo | Free test number (no voice) | Paid Indian number required | Yes (via Vonage credentials) | Not applicable for trial | No voice calls on free number |
| SIP-Trunk (e.g., Global Call Forwarding) | 15-day free trial of trunk | $25 + monthly for local number (≈ $3) | Yes (BYO via SIP) | Depends on carrier; generally none | Trial period only |

---

## 8. Quick Checklist for Getting an Indian Number Working with Vapi  

1. **Choose a provider** (Twilio is simplest for a single number; SIP-trunk if you need many DIDs).  
2. **Create/ purchase the Indian number** (pay the minimal monthly fee if needed).  
3. **Collect provider credentials** (Twilio SID/Auth-Token or SIP username/password).  
4. **Import the number into Vapi** via the Dashboard → Phone Numbers → Import (or `/phone-numbers/import` API).  
5. **Assign the number to an assistant** for inbound calls or note its `phoneNumberId` for outbound API calls.  
6. **Implement consent script** in the assistant’s system prompt to satisfy TRAI consent rules.  
7. **Schedule outbound calls only within 09:00-21:00 IST** and ensure DND scrubbing.  
8. **Monitor call health** via Vapi’s dashboard; if you hit outbound caps, consider upgrading the provider plan.

Following these steps lets you use an Indian virtual number with Vapi’s voice-AI platform while staying within the technical and regulatory constraints that currently apply.

---

### Sources
- [1] https://vapi.ai/blog/free-telephony-with-vapi
- [2] https://docs.vapi.ai/phone-calling
- [3] https://help.twilio.com/articles/360036052753-Twilio-Free-Trial-Limitations
- [4] https://help.twilio.com/articles/223136107-How-does-Twilio-s-Free-Trial-work
- [5] https://www.twilio.com/en-us/phone-numbers
- [6] https://www.plivo.com/virtual-phone-numbers/pricing/in/
- [7] https://plivo.com/docs/numbers
- [8] https://generalbytes.atlassian.net/wiki/spaces/ESD/pages/2974253058/Vonage+%28Nexmo%29+for+SMS+Notifications
- [9] https://www.vonage.com/communications-apis/phone-numbers/
- [10] https://www.3cx.com/partners/sip-trunks/india/
- [11] https://frejun.com/top-11-sip-trunk-providers-in-india/
- [12] https://docs.vapi.ai/api-reference/phone-numbers/create
- [13] https://docs.vapi.ai/calls/outbound-calling
- [14] https://en.wikipedia.org/wiki/STIR/SHAKEN

Twilio is the better (and in practice the only viable) choice for an Indian phone number with Vapi right now.
### 1. The decisive constraint: Plivo + Vapi + India
Vapi’s own advanced SIP docs for Plivo explicitly say that **Indian phone numbers cannot be used with Plivo on Vapi due to TRAI requirements that SIP termination happen via an Indian server, which Vapi does not support.** [docs.vapi](https://docs.vapi.ai/advanced/sip/plivo)
This single line basically rules Plivo out for +91 numbers on Vapi, regardless of pricing.

So even though Plivo offers Indian virtual numbers and has clear India voice pricing, you **cannot** use those Indian numbers with Vapi today. [plivo](https://www.plivo.com/voice/pricing/in/)
### 2. Twilio: works with Vapi for India
Vapi has a first-class Twilio SIP/number integration guide that shows how to connect Twilio numbers and use them for inbound and outbound calls via Vapi assistants. [docs.vapi](https://docs.vapi.ai/advanced/sip/twilio)
Twilio supports Indian numbers and exposes them via the same console/API Vapi already integrates with, so the **BYO phone number → Twilio path is officially supported**. [edesy](https://edesy.in/tools/twilio-pricing-calculator)

For India specifically:

- Twilio offers Indian phone numbers (local/mobile) on a monthly rental, currently around a couple of USD per month. [edesy](https://edesy.in/tools/twilio-pricing-calculator)
- Outbound and inbound per‑minute pricing for India is competitive and well documented in their pricing pages and calculators. [twilio](https://www.twilio.com/en-us/voice/pricing/in)

Given that your goal is “Indian phone number that works with Vapi,” Twilio clears both requirements, while Plivo fails the integration requirement for India.
### 3. Cost / practicality vs Plivo
Purely on raw telephony pricing, Plivo’s India voice rates are in the same general low range (per‑minute costs in rupees) and also aimed at high‑volume use. [plivo](https://www.plivo.com/voice/pricing/in/)
But because Vapi cannot terminate Indian traffic over Plivo today, you’d only be able to use Plivo India numbers with some other PBX or custom SIP stack, not with Vapi. [docs.vapi](https://docs.vapi.ai/advanced/sip/plivo)

So even if Plivo is marginally cheaper per minute in some cases, that **doesn’t help for a Vapi‑based AI calling stack**.
### 4. Practical recommendation for your use case
Given all of the above, for a Vapi + India setup:

- Use **Twilio** to buy a single Indian local/mobile number, then import it into Vapi using the Twilio integration or BYO workflow. [edesy](https://edesy.in/tools/twilio-pricing-calculator)
- Do **not** plan on using an Indian Plivo number directly with Vapi until Vapi either adds India‑specific SIP termination for Plivo or updates their docs to remove that limitation. [docs.vapi](https://docs.vapi.ai/advanced/sip/plivo)

If you want, I can outline a concrete Twilio + Vapi setup plan (step-by-step, including which Twilio console pages to touch and what to put into the Vapi import form) tailored to your exact call volume and use case.
- [15] https://docs.vapi.ai/tcpa-consent
- [16] https://talk-q.com/outbound-call-regulations-in-india
- [17] https://runo.ai/blog/trai-regulations-india
