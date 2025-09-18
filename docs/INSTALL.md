# Install

**Prereqs**: n8n (v0.198+), LinkedIn credential with posting permission, OpenAI (or your LLM).

1. Import: `workflows/linkedin-thought-leader.workflow.json`
2. Set credentials on:
   - LinkedIn → your Person or Organization (admin).
   - OpenAI (or provider) for the Basic LLM nodes.
3. Open **Basic LLM: Write Post** → paste `prompts/post-writer.md`.
4. Ensure node order:
   LLM → Strip Inline Links → Extract & Sanitize → Validate URL → Merge Originals → Assemble Final Post → IF Needs Repair? → (Repair path) → Assemble Final Post (repair) → IF Repair Successful? → Final Polish (dedupe) → LinkedIn
5. Run once manually, confirm:
   - `valid_links ≥ 2`, `invalid_links = 0`
   - `human_post_clean` looks professional.
6. Enable the Scheduled Trigger.
