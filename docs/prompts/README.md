This folder contains all prompt text used by the n8n “LinkedIn Thought-Leadership AI Agent.”
Each prompt maps to a specific Basic LLM node in your workflow.

Files

post-writer.md — main writer prompt (creates structured draft with Sources + tags)

repair-sources.md — full Sources block replacement when links fail validation

rewrite-failing-sources.md — targeted replacement for only the failing bullets

| n8n Node (type)                         | When it runs                                        | Paste this file into **User message** | Output is consumed by                                                            |
| --------------------------------------- | --------------------------------------------------- | ------------------------------------- | -------------------------------------------------------------------------------- |
| **Basic LLM: Write Post** (Basic LLM)   | Every run                                           | `prompts/post-writer.md`              | Code nodes: Strip Inline Links → Extract & Sanitize Sources → …                  |
| **Repair Sources (LLM)** (Basic LLM)    | When **Needs Repair?** is TRUE (no/low valid links) | `prompts/repair-sources.md`           | Code: Inject Repaired Sources → re-parse/validate → Assemble Final Post (repair) |
| **Rewrite Failing Sources** (Basic LLM) | When some (not all) links fail in repair path       | `prompts/rewrite-failing-sources.md`  | Code: Apply Rewritten Bullets → re-parse/validate (repair v2)                    |

All other nodes (Code/HTTP/IF/LinkedIn) do not use prompts

Required model settings (per node)
1) Basic LLM: Write Post

Model: GPT-5 (or your chosen model)

Temperature: 0.6–0.8 (variety, still professional)

Top P: default

Max tokens: ~1000–1500 (enough for 220 words + sections)

Stop sequences: none

User message: paste prompts/post-writer.md

Variables expected from upstream Set/Data Store (optional but recommended):

Repair Sources (LLM)

Model: GPT-5 (or same as writer)

Temperature: 0.4–0.6 (precision)

Top P: default

Max tokens: ~400

User message: paste prompts/repair-sources.md

Input dependency: the item must include final_post (from Assemble node)

3) Rewrite Failing Sources

Model: GPT-5

Temperature: 0.4–0.6

Top P: default

Max tokens: ~300

User message: paste prompts/rewrite-failing-sources.md

Input dependency: the item must include bad_bullets (from Summarize Link Errors (repair))
...............................................................................................................................


topic (optional override)

recent_topics, recent_hooks, recent_posts (strings; comma/line-separated history)

................................................................................................................................

What each prompt enforces
**post-writer.md**

Professional tone targeting VP/Director+.

Topic/structure/hook rotation to avoid repetition.

Strict link policy: 2–3 direct, public, HTTPS sources in a Sources: section only (no links in body; no shorteners; no tracking; no paywalls).

Concrete mini use case with realistic metric proxy.

Output format with labeled sections for downstream parsing (we humanize later).

**repair-sources.md**

Replaces only the “Sources:” block.

Returns 2–3 direct, public HTTPS URLs (shorteners/paywalls forbidden).

**rewrite-failing-sources.md**

Rewrites only the failing bullets, preserving count and order.

..................................................................................................................................

Wiring checklist (prompts context)

1. **Basic LLM: Write Post uses post-writer.md.**
Output flows to:

Strip Inline Links (Body) (Code) → removes any inline URLs in body.

Extract & Sanitize Sources (Code) → parses Sources bullets, bans shorteners.

Validate URL (HTTP) → HEAD or tiny GET (Range: bytes=0-64), no redirects, full response, never error.

Merge Originals (Code) → keeps post, url, title alongside status/headers.

Assemble Final Post (Code) → shortener hard-ban + humanized output; emits valid_links/invalid_links.

IF: Needs Repair? → routes when valid_links < 2 OR invalid_links > 0.

2. **Repair path:**
   Repair Sources (LLM) uses repair-sources.md.

Inject Repaired Sources (Code) → swaps in new Sources block.

Re-parse/re-validate (repair nodes).

Assemble Final Post (repair) (Code) → emits fresh valid_links/invalid_links.

3. **IF: Repair Successful?**

TRUE → Final Polish (dedupe) (Code) → LinkedIn.

FALSE → Use Original Post (Code; forwards original assembled item) → Final Polish (dedupe) → LinkedIn
(Or fail safe to “Hold & Alert” if you prefer to block posting when still failing.)

4. **Final Polish (dedupe) (Code)**

Removes extra hashtag lines/labels, outputs one plain hashtag line.

Ensures one signature line.

Leaves Sources intact (hides if only “no valid…”).

LinkedIn Text/Message should use:
{{ ("" + ($json.human_post_clean || $json.human_post || $json.final_post || $json.post || ""))
   .replace(/\u200B/g,"").replace(/\r/g,"").trim().slice(0,2900) }}

.................................................................................................................................................

Guardrails & variables referenced in prompts

1. {{ $json.topic }} — optional forced topic.

2. {{ $json.recent_topics }}, {{ $json.recent_hooks }}, {{ $json.recent_posts }} — strings you provide from a Data Store/Set node to avoid repetition.

3. Sources must be public HTTPS and non-shortened; examples of allowed domains are listed in the prompts.

4. Never include URLs in the body (enforced by prompt + Strip Inline Links node

................................................................................................................................................
**Quick test (after pasting prompts)**

1. Run Basic LLM: Write Post → confirm draft has:

- labeled sections,

- a Sources block with 2–3 items,

- no URLs in the body (after Strip Inline Links).

2. Run through Validate URL → Assemble Final Post:

- Check valid_links ≥ 2, invalid_links = 0.

If not, ensure repair branch fires and Assemble Final Post (repair) meets the same criteria.

3. Final Polish (dedupe) output should be:

- no labels like “Title/Hook/Hashtags,”

- one plain hashtag line,

- one signature,

- Sources present with working, direct URLs.

................................................................................................................................................

**Common pitfalls**

- Shorteners sneaking in: make sure HTTP node has Follow Redirects: OFF (or max redirects 0) and Assemble code includes the shortener hard-ban.

- Posted with 0 sources: in Needs Repair? route TRUE if valid_links < 2 or invalid_links > 0. In Repair Successful?, require valid_links ≥ 2 and invalid_links = 0.

- Duplicate hashtags/signature: ensure Final Polish (dedupe) is the last node before LinkedIn, and LinkedIn uses human_post_clean.

..................................................................................................................................................

**Versioning prompts**


When you change any prompt:

1. Update the file in prompts/.

2. Note the change in CHANGELOG.md.

3. Consider bumping a minor version tag (e.g., v1.1.0) and attach the updated workflow JSON.
