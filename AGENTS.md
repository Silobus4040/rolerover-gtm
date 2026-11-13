# RoleRover GTM - Agent Operational Guidelines & Architecture Memory

## 1. Mandatory Documentation Protocol
Before taking any implementation or modification actions, **ALWAYS consult official documentation**:
* **n8n Documentation:** [https://docs.n8n.io/](https://docs.n8n.io/)
  - Always design workflows taking the fastest, most performant route:
    * Use native **SplitInBatches** with concurrency rather than single-threaded serial execution.
    * Use **Sub-Workflows (`Execute Workflow`)** in parallel execution mode for high-throughput batch qualification.
    * Use native data table bulk operations and cursor pagination as specified by the n8n API reference.
* **Cloudflare Workers AI:** [https://developers.cloudflare.com/workers-ai/](https://developers.cloudflare.com/workers-ai/)
  - Model mandates: `@cf/openai/gpt-oss-20b` for Fast Triage; `@cf/meta/llama-3.3-70b-instruct-fp8-fast` for Deep Classification & Copy Drafting.
* **Supabase / GoTrue:** [https://supabase.com/docs/](https://supabase.com/docs/)
  - Connected to Hetzner self-hosted instance (`https://core-ffnrirrrrnwoo3837q2b9x.myrolerover.com`).

---

## 2. Scraping & Lead Qualification Pipeline Rules
* **Context Over Blind Disqualification:**
  - Mentions of *"link in comments"* or *"join my telegram channel"* are **never** dropped without context. Evaluate the entire message first.
  - Drop **only** if the context confirms a pay-to-earn micro-task scheme (e.g. *basic plan for $3*, *tasks = $6*, *registration free task*, *minimum withdrawal $5*, or paid referral traps).
* **Safety Net (`unsure`):**
  - If the model is not 100% certain whether an item is a genuine seeker or a task scheme, it must **never discard it**—it marks it as `unsure` (`Needs Review`), ensuring zero false negatives.
* **UGC Reply Rules:**
  - **Unknown Role / Context:** Generate natural peer discovery question asking what roles they target. **DO NOT** pitch `@myrolerover` yet.
  - **Known Role or Pain Point:** Generate the proven PAS structure with `@myrolerover` and subtle free trial note.

---

## 3. High-Throughput Processing Mandate
* Never run long batch operations in a single-threaded blocking loop when parallel asynchronous calls (`Promise.all` concurrency pools) can finish the task 5x–10x faster.
