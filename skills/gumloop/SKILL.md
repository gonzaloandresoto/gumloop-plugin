---
name: gumloop
description: Conventions for every call to the gumloop MCP server (sign-in, team scope, confirmations, error shape, reply format). Use whenever a gumloop tool is about to be called, alongside the task-specific Gumloop skill.
---

# Gumloop conventions

## Sign-in

The client signs the user in through Gumloop's browser consent flow on first use. Never ask for an API key or token. When every tool fails with an authentication error, tell the user to reconnect the `gumloop` server in their client.

## Team scope

Tools that accept `project_id` default to the personal workspace. When the user works in a team, call `list_teams` once and pass the chosen `team_id` as `project_id` on every later call. A 403 saying a tool "is not available on a Gumloop MCP connection scoped to a team" is final; report it, don't retry.

## Before calling

- Take IDs and tool names from `list_agents`, `list_teams`, `list_mcp_servers`, and `list_mcp_server_tools`, never from memory.
- Ask the user before a call that creates, updates, deletes, cancels, or retires anything, spends credits (an agent session, an evaluation), acts in another app, or reaches another person.

## Results

- `create_agent_session` and `send_session_message` return when the agent finishes. After a client timeout, `get_session` has the result.
- An error is a tool result shaped `{"error_status", "message", "traceback"}`. Report the tool name, the ID or query, and `message`. Omit `traceback`.
- Reply with names, IDs, states, and Gumloop URLs. No raw payloads unless the user asks for JSON.

## Terminal

In a client with a shell (Claude Code, Codex, Cursor), the `gumloop` CLI reaches the same account. Use it when the user asks for the CLI, when output must be piped or scripted, or when the `gumloop` MCP server isn't connected. Check `gumloop --version`; install with `curl -fsSL https://gumloop.com/cli/install.sh | sh` (macOS and Linux only), then `gumloop plugin install gumloop` installs the `gumloop-cli` skill with the full command reference. Read that skill before using unfamiliar commands.

## Where to send the user

| For | Link |
| --- | --- |
| Connect or manage apps | https://www.gumloop.com/settings/profile/connectors |
| Help center | https://docs.gumloop.com/help |
| Support | support@gumloop.com |
| Service status | https://status.gumloop.com |
| Security and compliance | https://trust.gumloop.com |

## Admin and docs tools

Available on a personal connection to an organization admin. `organization_id` comes from `list_organizations`.

| Need | Tools |
| --- | --- |
| Audit trail | `get_audit_logs` |
| CSV export | `export_data`, then `get_export_status` |
| Team membership | `manage_team_members` |
| Custom role membership and credit limits | `manage_custom_role_members`, `list_custom_role_credit_limits`, `get_custom_role_credit_limit`, `set_custom_role_credit_limit` |
| Questions about Gumloop itself | `search_documentation`, `ask_gummie` |
