---
name: connected-apps
description: Reads from or acts in apps the user connected to Gumloop (Gmail, Google Calendar, Slack, Linear, Notion, HubSpot, Salesforce, GitHub, and 100+ more) through the gumloop server's tools. Use when a task needs data or an action in one of those apps and the user has Gumloop, when the user says "through Gumloop", or when they ask which apps are connected.
---

# Connected apps

The user's app sign-ins live in Gumloop. An app with status `connected` is usable now, with the user's own sign-in and under their organization's policy. Never ask for an app credential, and never set the app up yourself.

## Steps

1. `list_mcp_servers` (`project_id` for a team). Keep the list for the conversation. Each entry has `server_id`, `name`, `status`, `tool_count`, and `gumloop_auth_url`.
2. `list_mcp_server_tools` (`server_id`). Read the `input_schema` of the tool you need.
3. `call_mcp_tool` (`server_id`, `tool_name`, `arguments`). Read `results[0]`: `status` `success` with `content`, or `error`.

When two connected apps fit the request (two calendars, two chat tools), ask which.

## Status

| `status` | Do |
| --- | --- |
| `connected` | Call its tools. |
| `unauthenticated` | Hand the user the entry's `gumloop_auth_url` and retry after they connect. You can't do this step. |
| `blocked` | Say org policy restricts the app for this user. Stop. |

Mid-call, a result with `status: "unauthenticated"` and `error.code: "auth_required"` is the same case; the link is `error.details.gumloop_auth_url`. `server_blocked` and `tool_blocked` are policy. `mcp_tool_error` carries the app's own message; show it.

When an app is `unauthenticated` and the user works in a team, retry `list_mcp_servers` with the team's `project_id` before sending them to connect.

## Writes

Reads (list, search, get, fetch) need no confirmation. Before a tool that sends, posts, creates, updates, or deletes in the app, show the user what will go out and to whom, and wait for a yes. Judge by the tool's name and description; when unsure, treat it as a write.

## Resources and prompts

`list_mcp_resources`, `read_mcp_resource` (`uri`), `list_mcp_prompts`, `get_mcp_prompt`. Only when no tool fits.
