This is one of the automation pipelines I designed for content intelligence + autonomous publishing workflows, similar to what we use in AI-driven monitoring systems at Hypotenuse Analytics — except here the domain is mining news instead of surveillance or infrastructure data.

The goal of this workflow is simple:

1. Automatically monitor a live RSS source
2. Extract fresh industry news
3. Use GPT to convert raw news into SEO-optimized long-form articles
4. Enrich the content with fallback media handling
5. Push everything directly into Sanity CMS
6. Keep the article as draft for human review

This is basically an AI newsroom pipeline.

---

## HIGH LEVEL FLOW

Schedule Trigger
↓
Fetch RSS Feed
↓
Convert XML → JSON
↓
Split Articles
↓
Limit Items
↓
Generate AI Blog with OpenAI
↓
Parse Structured JSON
↓
Validate/Fallback Image
↓
Merge Clean Data
↓
Publish to Sanity CMS

---

1. SCHEDULE TRIGGER

---

I used the Schedule Trigger node to run the workflow every 12 hours.

Why?

Because news websites update continuously and I wanted an automated ingestion cycle without manually running anything.

This turns the workflow into a self-operating content engine.

Inside the trigger:

* interval field = hours
* hoursInterval = 12

Meaning:
The workflow executes twice daily automatically.

In production systems, this is exactly how autonomous monitoring agents work.

---

2. FETCHING THE RSS FEED

---

Node: HTTP Request2

This node pulls data from:

https://www.mining.com/feed/

I also added a browser User-Agent header because many news sites block requests coming from unknown bots.

Header used:

User-Agent:
Mozilla/5.0 ...

This small detail prevents feed rejection.

Very important in scraping + automation systems.

---

3. XML TO JSON CONVERSION

---

RSS feeds return XML.

n8n works better with JSON.

So I used the XML node to transform the feed into structured JSON.

Now the data becomes machine-readable and downstream AI nodes can process it cleanly.

---

4. SPLIT OUT NODE

---

RSS feeds contain many articles inside one payload.

The Split Out node extracts:

rss.channel.item

This converts:

1 feed with many articles
→ into
multiple individual execution items

Now every article becomes independently processable.

This is critical because GPT should process one article at a time.

---

5. LIMIT NODE

---

I added a Limit node before GPT generation.

Why?

Because LLM calls cost money.

Without limits:

* huge RSS feeds
* unnecessary token usage
* higher latency

In production AI systems, controlling throughput is extremely important.

This node acts like:

* rate limiting
* budget control
* inference optimization

---

6. OPENAI CONTENT GENERATION

---

This is the brain of the workflow.

Node: OpenAI (HTTP Request)

Instead of using a prebuilt node, I directly hit:

https://api.openai.com/v1/chat/completions

Why I prefer this sometimes:

* full control over payload
* easier debugging
* model flexibility
* production portability

Model used:
gpt-4o-mini

The prompt engineering here is the important part.

I forced the model to:

* return ONLY raw JSON
* generate 800+ words
* create SEO title
* generate excerpt
* generate HTML-formatted content
* generate tags array
* maintain journalistic tone

This structure is extremely important because AI output must be deterministic for automation pipelines.

Bad prompting breaks production systems.

---

7. STRUCTURED JSON PARSING

---

After GPT responds, the content is still a raw string.

So I used:

JSON.parse()

inside Set nodes to extract:

* title
* excerpt
* content
* tags

This converts unstructured AI output into structured workflow variables.

Very important concept:

AI alone is not automation.

Structured AI output is automation.

---

8. IMAGE FALLBACK SYSTEM

---

This is one of the smartest parts of the workflow.

I check:

Does the article contain an image?

Using the IF node:

* if image exists → use it
* else → inject default fallback image

Why this matters:

CMS rendering breaks when media fields are empty.

A fallback system guarantees:

* visual consistency
* no broken thumbnails
* no empty UI cards

This is production-grade defensive workflow design.

---

9. MERGE NODE

---

After validation, both branches are merged.

This ensures:

* one clean payload
* standardized schema
* stable downstream publishing

This is basically data normalization before persistence.

---

10. SANITY CMS PUBLISHING

---

Final node pushes the article into Sanity CMS using:

/data/mutate/production

I create:

* title
* slug
* image
* excerpt
* content
* tags
* draft status
* published timestamp

Important detail:

I intentionally publish as:
status = draft

Why?

Because fully autonomous publishing without review is dangerous.

Human-in-the-loop systems are safer.

Especially in:

* journalism
* AI content
* compliance-heavy industries

---

## WHY THIS WORKFLOW IS POWERFUL

This pipeline is not just "AI blogging".

It demonstrates:

1. Autonomous ingestion systems
2. AI transformation pipelines
3. Structured output engineering
4. CMS orchestration
5. Fault tolerance
6. Production automation patterns
7. Human-review safety layers

This exact architecture pattern can be adapted for:

* surveillance alerts
* SHM anomaly reports
* cybersecurity intelligence
* satellite event summaries
* deepfake detection reporting

At Hypotenuse Analytics, the same philosophy applies:

Raw signals
→ AI interpretation
→ structured intelligence
→ automated delivery

Only the data source changes.

---

## BIGGEST LESSON

Most beginners think automation means:
“connect nodes”

No.

Real automation means:

* reliable outputs
* defensive handling
* structured schemas
* scalable execution
* failure prevention
* deterministic AI behavior

That is what makes workflows production-ready.

---

## WHAT I WOULD IMPROVE NEXT

If I scale this further, I would add:

1. Duplicate article detection
2. Vector database memory
3. SEO keyword enrichment
4. AI-generated featured images
5. Multi-language generation
6. Auto social media posting
7. Semantic topic clustering
8. Human approval dashboard
9. Retry + dead-letter queues
10. Analytics tracking

That turns this from automation
into a full AI media operating system.
