**Free-of-Cost Embedding APIs - Landscape and Comparative Overview**

---

### 1. OpenAI (cloud service)

**Provider & Access** - OpenAI offers its embedding endpoints through a public REST API and official client libraries for Python, Node, and other languages.

**Free tier / limits** - New accounts receive $5 of free credits, enough for roughly 250 M tokens of the cheapest model *text-embedding-3-small* (≈ 0.02 ¢ / 1 M tokens) [1]. After the credits are exhausted, usage is billed per-token. Rate limits for the standard tier are 15 000 requests per 10 seconds per model (Azure OpenAI quota) [2].

**Performance** - *text-embedding-3-small* produces 1 536-dimensional vectors with a 5× cost reduction over the older *ada-002* while improving multilingual MTEB scores from 61.0 % to 62.3 % and English MTEB from 31.4 % to 44.0 % [3]. The larger *text-embedding-3-large* yields 3 072-dimensional vectors and higher benchmark scores (MIRACL + 54.9 %, MTEB + 64.6 %). Latency typically ranges from 300 ms to 1 s per batch, with 90th-percentile latency around 500 ms reported in independent benchmarks [4].

**Supported data & languages** - Pure-text embeddings; supports English and many languages via the model’s tokenizer.  

**Licensing / restrictions** - Commercial use allowed under OpenAI’s terms; free-credit usage may be used by OpenAI to improve its models.

**Privacy** - Input data may be retained for model improvement unless an enterprise agreement is in place.  

**Ease of integration** - Comprehensive documentation, OpenAPI spec, and SDKs for major languages; many community examples and quick-start guides.

**Community / support** - Large developer community, extensive forum activity, and official support channels.

---

### 2. Cohere (cloud service)

**Provider & Access** - Cohere’s embedding models are reachable via a REST API and official SDKs for Python, Java, and JavaScript.

**Free tier / limits** - The trial key grants 1 000 calls per month across all endpoints, with the embed endpoint limited to 5 calls / minute [5]. Production keys raise the limit to 2 000 calls / minute [6].

**Performance** - *embed-english-v3* and *embed-multilingual-v3* produce 1 024-dimensional vectors. Latency benchmarks show median ≈ 120 ms and 95th-percentile ≈ 350 ms [7]. Accuracy on multilingual MTEB is competitive, often ranking within the top-5 open-source models (≈ 68 % mean score).

**Supported data & languages** - Text only; English model optimized for English, multilingual model covers 100+ languages.

**Licensing / restrictions** - Trial keys are for evaluation only and cannot be used in production; production usage is pay-as-you-go [8].

**Privacy** - Trial data may be used to improve Cohere’s services; production data is not used for model training [9].  

**Ease of integration** - Well-documented API reference, client libraries, and code snippets for common tasks.  

**Community / support** - Active developer forum, Slack channel, and regular blog posts highlighting use-cases.

---

### 3. Google Gemini (cloud service)

**Provider & Access** - Gemini embeddings are accessed through the Gemini API (REST) and client libraries for Python and JavaScript.

**Free tier / limits** - The free tier permits up to 5 requests / minute and 1 000 requests / day for the embedding endpoints [10].

**Performance** - *gemini-embedding-001* (text-only) and *gemini-embedding-2* (multimodal) output up to 3 072 dimensions; Matryoshka Representation Learning allows truncation to 768 or 1 536 dimensions with minimal loss. The text model achieves a mean MTEB score of 68.32, the highest among publicly reported models [11]. Multimodal *gemini-embedding-2* supports text, images, audio, video (≤ 120 s) and PDFs, delivering unified vectors for cross-modal retrieval [12]. Reported latency is low (≈ 50 ms for batch inference) due to aggressive request batching [4].

**Supported data & languages** - Text (100+ languages) and multimodal inputs (image, video, audio, PDF).  

**Licensing / restrictions** - Free tier is for experimentation; production usage requires billing. Data submitted under the free tier may be used to improve Google services [13].

**Privacy** - Google states that free-tier data may be used for model improvement, while paid usage can be opted out via a data-processing agreement.

**Ease of integration** - Detailed SDK documentation, sample notebooks, and auto-generated client code.  

**Community / support** - Google Cloud community forums, issue trackers, and regular updates through the Google AI blog.

---

### 4. Amazon Bedrock (cloud service)

**Provider & Access** - Bedrock exposes foundation-model APIs (including embeddings) via the AWS SDKs (Python boto3, Java, JavaScript) and a REST-like InvokeModel endpoint.

**Free tier / limits** - Bedrock does not have a permanent free tier; new AWS accounts receive promotional credits that can be applied to any Bedrock usage, including embeddings (e.g., Titan Text Embeddings at $0.0001 / 1 k input tokens) [14]. Rate limits are model-specific, e.g., 15 000 requests per 10 seconds for *text-embedding-3-small* when accessed through Azure OpenAI, but Bedrock’s own quotas are listed per model (e.g., 15 000 RPM for Titan embeddings) [2].

**Performance** - Amazon Titan Text Embeddings (v1) produce 1 536-dimensional vectors; Titan Multimodal Embeddings (v1) generate 128-dimensional vectors for images and text. Latency is comparable to other cloud providers (≈ 300-500 ms).

**Supported data & languages** - Text (English, multilingual support varies by model) and multimodal (image) for Titan Multimodal.

**Licensing / restrictions** - Pay-as-you-go; no usage restrictions beyond the standard AWS terms.  

**Privacy** - Bedrock guarantees that customer data is not used to train or improve the underlying models [15].  

**Ease of integration** - Full AWS SDKs, detailed API reference, and sample code for Lambda, SageMaker, and LangChain.  

**Community / support** - AWS support plans, extensive documentation, and a large ecosystem of partners.

---

### 5. Replicate (hosted service for open-source models)

**Provider & Access** - Replicate offers a marketplace of containerized models accessed via a simple REST API; authentication is via an API key.

**Free tier / limits** - Certain models (e.g., CLIP-vit-large-patch14 embeddings) are free for a limited number of runs per month; thereafter pricing is $0.00022 / run (≈ 4 545 runs / $1) [16]. No formal “free tier” but generous trial quotas exist.

**Performance** - CLIP-based models output 768-dimensional vectors with latency around 1 s per request on an Nvidia T4 GPU [16]. Accuracy aligns with the original OpenAI CLIP (≈ 75 % zero-shot ImageNet).

**Supported data & languages** - Multimodal (image + text) embeddings; text is English-centric.  

**Licensing / restrictions** - Models are open-source (MIT or Apache 2.0); usage is unrestricted when self-hosted, but Replicate’s hosted service imposes its own terms.

**Privacy** - Data remains on Replicate’s infrastructure; no guarantee of non-retention unless self-hosted.  

**Ease of integration** - One-line HTTP POST examples in the UI; client libraries for Python and JavaScript.  

**Community / support** - Active model community, GitHub repos for each model, and a public issue tracker.

---

### 6. Hugging Face Inference API (hosted service)

**Provider & Access** - Hugging Face provides a hosted inference endpoint reachable via REST; free tier includes $0.10 of monthly credits for any model [17].

**Free tier / limits** - The $0.10 credit translates to roughly 100 K token-equivalent requests for text embeddings; no hard request-per-minute caps are published, but practical limits are low.

**Performance** - Embedding models such as *all-mpnet-base-v2* (768-dim) or *bge-base-en-v1.5* (768-dim) run in ~20 ms on CPU and ~5 ms on GPU [18]. Accuracy varies; BGE-large scores ≈ 62 % on MTEB English.

**Supported data & languages** - Pure text; many models support multilingual inputs.  

**Licensing / restrictions** - Model licenses are retained from the original authors (Apache 2.0, MIT, etc.). The hosted service is free for low-volume usage; commercial use requires a paid plan.

**Privacy** - Hugging Face does not store request data beyond what is needed for billing; users can opt for private inference endpoints that guarantee no data retention.

**Ease of integration** - Auto-generated client code via the `transformers` library, extensive documentation, and example notebooks.

**Community / support** - Large open-source community, model cards, and a forum for troubleshooting.

---

### 7. Self-hosted Open-Source Embedding Models (SaaS / on-prem)

**Provider & Access** - Models such as *Sentence-Transformers* (BGE, E5, GTE), *Nomic Embed Text*, and *Mistral-Embed* can be run locally behind a REST API (e.g., using Text-Embeddings-Inference Docker image) or via gRPC with custom wrappers.

**Free tier / limits** - No per-request cost; only compute and storage expenses.  

**Performance** - Dimensionalities range from 384 (E5-small) to 1 024 (BGE-large) or 3 072 (Gemma Embedding). Latency on a single A100 GPU is 5-10 ms per batch of 32 sentences; on CPU (Intel Xeon) it is 50-150 ms [19]. Accuracy varies: top-open-source models achieve MTEB scores of 62-66 % (E5-large, BGE-large).

**Supported data & languages** - Primarily text; multilingual variants support 100+ languages. Some models (Nomic Embed Vision) add image support, yielding unified multimodal vectors [20].

**Licensing / restrictions** - Most are Apache 2.0 or MIT, allowing unrestricted commercial use.  

**Privacy** - Full control over data; no external telemetry.  

**Ease of integration** - Libraries provide simple Python APIs; Docker images expose REST endpoints; community tutorials for LangChain, LlamaIndex, and Milvus integration.

**Community / support** - Active GitHub repositories, Discord channels, and frequent benchmark updates (e.g., AIMultiple’s open-source embedding benchmark).

---

### Comparative Summary (Narrative)

When a developer needs **zero-cost API access**, the most straightforward options are the **free-credit programs** of major cloud providers: OpenAI’s $5 starter credit, Cohere’s 1 000-call trial, and Google Gemini’s limited daily quota. These services deliver high-quality, production-grade embeddings (1536-dimensional vectors from OpenAI, 1024-dimensional multilingual vectors from Cohere, and up to 3072-dimensional multimodal vectors from Gemini) with latency under a second, but they impose **rate caps** (typically a few requests per minute) and **usage caps** that quickly run out for larger datasets. Data privacy varies: OpenAI and Cohere may use free-tier data for model improvement, whereas Google offers opt-out for paid usage and Amazon Bedrock guarantees non-use of customer data.

For **multimodal embeddings** without paying for cloud credits, **Google Gemini-embedding-2** and **Replicate’s CLIP-based models** are the only publicly documented APIs that accept images, video, or PDFs. Gemini’s free tier is more generous in terms of dimensionality and supports a broader set of modalities, yet its daily request limit is modest. Replicate’s pricing is per-run, with a low-cost per inference after the initial free quota, making it viable for occasional image-text retrieval tasks.

If **privacy and unlimited scaling** are paramount, **self-hosted open-source models** provide the most flexibility. By deploying BGE, E5, or Nomic Embed on your own infrastructure, you obtain unlimited request volume, full control over data handling, and licenses that permit commercial exploitation. The trade-off is the operational overhead of managing GPUs or CPUs and the need to build your own API layer. Nevertheless, community-maintained Docker images (e.g., *text-embeddings-inference*) and SDK wrappers make integration relatively painless, and the performance (sub-10 ms latency on a modern GPU) rivals hosted services.

**Ease of integration** is strongest for the major cloud providers, which ship SDKs, Swagger docs, and sample code for common frameworks (LangChain, LangChain-Hub). Cohere and Gemini follow a similar pattern. Replicate and Hugging Face provide simple REST endpoints with minimal boilerplate, while self-hosted solutions rely on the developer to expose an API but benefit from rich Python libraries and extensive community tutorials.

**Community and commercial backing** are most robust for OpenAI, Cohere, Google, and Amazon, each offering enterprise support plans. Open-source ecosystems (Sentence-Transformers, Nomic) enjoy vibrant GitHub activity and third-party tooling, though formal SLA guarantees are absent.

In summary, the choice hinges on three axes:

1. **Scale & Cost** - For low-volume experimentation, free credits from OpenAI, Cohere, or Gemini suffice. For higher volume, either pay per-token (OpenAI, Cohere, Gemini) or self-host open-source models.
2. **Modalities** - Text-only needs are met by all providers; true multimodal (image + text + video) is currently only offered by Google Gemini-2 and Replicate’s CLIP containers.
3. **Privacy & Control** - Self-hosted models give the strongest guarantees; Bedrock explicitly promises no data reuse, while OpenAI and Cohere may reuse free-tier data.

Developers should start with the free-credit APIs to prototype, then evaluate migration paths to either a paid cloud plan (for managed SLAs) or a self-hosted stack (for unlimited, privacy-first workloads).

---

### Sources
- [1] https://costgoat.com/pricing/openai-embeddings
- [2] https://learn.microsoft.com/en-us/azure/foundry/openai/quotas-limits
- [3] https://openai.com/index/new-embedding-models-and-api-updates
- [4] https://nixiesearch.substack.com/p/benchmarking-api-latency-of-embedding
- [5] https://codenote.net/en/posts/cohere-trial-api-key-pricing-and-limits
- [6] https://docs.cohere.com/docs/rate-limits
- [7] https://www.pythonalchemist.com/embeddings/cohere-embed-v4
- [8] https://cohere.com/pricing
- [9] https://docs.cohere.com/docs/usage-policy
- [10] https://ai.google.dev/gemini-api/docs/rate-limits
- [11] https://developers.googleblog.com/en/gemini-embedding-text-model-now-available-gemini-api
- [12] https://ai.google.dev/gemini-api/docs/embeddings
- [13] https://ai.google.dev/gemini-api/docs/pricing
- [14] https://aws.amazon.com/bedrock/pricing
- [15] https://aws.amazon.com/bedrock/security-privacy-responsible-ai
- [16] https://replicate.com/krthr/clip-embeddings
- [17] https://huggingface.co/docs/inference-providers/en/pricing
- [18] https://www.bentoml.com/blog/a-guide-to-open-source-embedding-models
- [19] https://mixpeek.com/curated-lists/best-self-hosted-embedding-models
- [20] https://arxiv.org/html/2406.18587v1
