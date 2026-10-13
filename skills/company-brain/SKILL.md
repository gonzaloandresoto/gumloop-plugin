---
name: company-brain
description: Searches the company knowledge indexed in Gumloop's Company Brain (Notion, Google Drive, Slack, GitHub, Confluence, uploaded files, agent outputs). Use when the user asks what the company knows, decided, or documented about something, where a policy or process is written down, or says "Company Brain".
---

# Company Brain

`search_brain` with `query`, `max_limit` (default 8, max 50), and optional `source_type` from `notion`, `google_drive`, `slack`, `github`, `confluence`, `direct_file_uploads`, `gumloop_artifacts`.

Answer from each result's `content`. Cite `title` with `url`. When results disagree, trust the newer `updated_at`. Search once more with different wording before reporting nothing found.

Each search spends credits and needs the Pro plan. On a plan error, say the feature isn't in the user's plan.
