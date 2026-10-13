---
name: get-started
description: Checks a fresh Gumloop connection and shows what the account has. Use right after installing the Gumloop plugin, or when the user asks to connect Gumloop, test it, or see their agents and connected apps.
---

# Get started

Run in order. Stop at the first failure.

1. `list_teams`. On an authentication error, ask the user to connect the `gumloop` server in their client, which opens Gumloop's sign-in in the browser. Then stop.
2. With more than one team, ask which to use by default and keep its `team_id` as `project_id`.
3. `list_agents`. Name up to five agents and what each does.
4. `list_mcp_servers`. Name the apps with status `connected`. Give the count of `unauthenticated` apps and say each connects from its `gumloop_auth_url`.
5. Offer three next steps drawn from what exists: run one of the agents, read something from a connected app, search Company Brain.

With no agents and no connected apps, point the user to https://www.gumloop.com to build a first agent.
