# n8n LinkedIn Thought-Leadership AI Agent

Automates professional LinkedIn posts with **validated, non-shortened sources**, a **repair loop** for broken links, and a **human tone** (no botty labels).

[![Repo QA](https://github.com/pratapsfdc22-dev/n8n-linkedin-thought-leadership-AI-Agent/actions/workflows/release.yml/badge.svg?branch=main)](https://github.com/pratapsfdc22-dev/n8n-linkedin-thought-leadership-AI-Agent/actions/workflows/release.yml)
[![GitHub release](https://img.shields.io/github/v/release/pratapsfdc22-dev/n8n-linkedin-thought-leadership-AI-Agent?display_name=tag)](https://github.com/pratapsfdc22-dev/n8n-linkedin-thought-leadership-AI-Agent/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## What it does
- **Writes** 140–220 word thought-leadership posts with topic/structure rotation.
- **Validates** sources (HEAD/tiny GET), **bans shorteners**, auto-repairs broken links.
- **Humanizes** the draft (no “Title/Hook/Hashtags” labels), **dedupes** hashtags/signature.
- **Safety gates**: won’t publish unless sources pass (configurable).

## Quickstart
1. Import `workflows/linkedin-thought-leader.workflow.json` into n8n.
2. Set credentials for **LinkedIn** (Person/Org) and your **LLM**.
3. In **Basic LLM: Write Post**, paste `prompts/post-writer.md`.
4. Run top→bottom once; confirm `valid_links ≥ 2`, `invalid_links = 0`.
5. Enable the **Scheduled Trigger**.

**Docs:** [`docs/INSTALL.md`](docs/INSTALL.md) • **Prompts:** [`prompts/`](prompts/) • **Workflow:** [`workflows/`](workflows/)  
**CI:** [Actions](https://github.com/pratapsfdc22-dev/n8n-linkedin-thought-leadership-AI-Agent/actions) • **Releases:** [Releases](https://github.com/pratapsfdc22-dev/n8n-linkedin-thought-leadership-AI-Agent/releases)





# n8n LinkedIn Thought-Leadership Auto-Poster

Automates professional LinkedIn posts with **validated, non-shortened sources**, a **human tone**, and a **repair loop** that replaces broken links before publishing.

## What it does
- **Writes** 140–220 word thought-leadership posts with topic/structure rotation.
- **Validates** links (HTTP HEAD / tiny GET), **bans shorteners**, auto-repairs Sources.
- **Humanizes** the draft (no “Title/Hook” labels), **dedupes** hashtags/signature.
- **Safety gates**: won’t publish unless sources are valid (configurable).

## Quickstart (5 minutes)
1. Import `workflows/linkedin-thought-leader.workflow.json` into n8n.
2. Add your **OpenAI** (or model) and **LinkedIn** credentials (kept in n8n).
3. Open node **Basic LLM: Write Post** → paste the prompt from `prompts/post-writer.md`.
4. Run the chain top→bottom once; confirm `valid_links ≥ 2` and preview `human_post_clean`.
5. Set **Schedule** and enable.

## Architecture
See `docs/ARCHITECTURE.md` and the canvas screenshot in `assets/architecture.png`.

## Demo
- 3-min walkthrough video: (add link)
- Example output: `workflows/examples/sample-output.md`

## License
MIT (see `LICENSE`). For commercial licensing, see `EULA.md` (optional).
