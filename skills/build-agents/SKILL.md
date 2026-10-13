---
name: build-agents
description: Creates a Gumloop agent or changes its model, system prompt, skills, connected apps, or evaluation settings. Use when the user wants a new agent, wants an agent to use an app such as Gmail or Slack, wants to attach or write a skill, or asks how an agent has been scoring.
---

# Build agents

## Create or update

1. `list_models` for valid `model_name` values.
2. `get_agent` on an existing agent and copy its `tools` and `resources` arrays for the shape. Don't invent it.
3. Show the user the configuration, then `create_agent` (`name`, `model_name`, `description`, `system_prompt`, `skill_ids`, `project_id`) or `update_agent` (`agent_id` and the changed fields).

`update_agent` with `is_active: false` retires the agent for good; later calls return 404. To pause autonomous runs, the user disables the agent's triggers in Gumloop.

`list_agent_versions` and `get_agent_version` answer "what changed".

## Skills

`list_skills` (`search_query`, `agent_id`, `related_server_id`), then `attach_agent_skills` or `detach_agent_skills` with `skill_ids`.

`create_skill` and `update_skill` take `file_names` already in the user's Gumloop workspace storage, not local files; `from_workspace: true` reads team storage. `update_skill` replaces all files, so pass the full set. `download_skill` returns an archive URL. `delete_skill` is permanent; ask first.

## Connected apps on the agent

`list_agent_mcp_servers` (`agent_id`). `attach_agent_mcp_server` (`agent_id`, `server_id` from `list_mcp_servers`, optional `approval_mode`, `restricted_tools`) adds or reconfigures an app. In its response, `auth_status` `unauthenticated` means the user still has to connect the app and `blocked` means org policy forbids it. `detach_agent_mcp_server` removes it.

## Evaluations

`get_evaluation_options` for allowed values, then `get_agent_evaluation_config` and `update_agent_evaluation_config`. Read results with `list_agent_evaluations`, `get_agent_evaluation`, and `get_agent_evaluation_metrics` (`days`). `run_agent_evaluations` (`session_ids`) spends credits; ask first, or pass `dry_run: true`.
