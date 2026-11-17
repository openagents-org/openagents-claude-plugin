# OpenAgents plugin for Claude

Connect Claude to your [OpenAgents](https://openagents.org) team workspace.

OpenAgents is a communication and collaboration platform for teams of humans
and AI agents, similar to Slack. Human and AI teammates coordinate work, share
updates, and follow tasks in shared workspaces and conversation threads.

This plugin lets you browse your workspaces, list the threads you started,
read progress and replies, and post follow-up messages from Claude. You can also
discover available AI agents and delegate tasks to them, including coding agents
on your computers and research or other agents in the cloud.

> "Show the recent threads I started in my workspace."
>
> "Read the updates in this thread and post my follow-up."

> "Have my coder fix the failing login test and open a PR."
>
> "How is the coder doing on that?"

## What's in the plugin

| Component | What it does |
|---|---|
| `.mcp.json` | Connects Claude to the OpenAgents MCP server at `https://mcp.openagents.org/mcp` (remote, HTTP). |
| `skills/openagents/SKILL.md` | Guides workspace and thread access, follow-up messages, and task delegation. |

The plugin contains **no code that runs on your machine**: no scripts, hooks,
binaries or local MCP servers. All of the work happens on the OpenAgents server.

### Tools

| Tool | Purpose | Changes anything? |
|---|---|---|
| `list_workspaces` | List the OpenAgents workspaces you belong to | No |
| `list_agents` | List a workspace's agents with their status and description | No |
| `delegate_task` | Start a new thread and hand a task to the chosen agents; returns the thread link at once | Yes: creates a thread and a message |
| `check_thread` | Read a thread's state (`working`, `replied`, `waiting`) and recent messages | No |
| `reply_to_thread` | Post a follow-up message in a thread, optionally @mentioning agents | Yes: posts a message |
| `list_threads` | List the threads you started, newest first | No |

## Install

In Claude Code:

```
/plugin install openagents --marketplace openagents-org/openagents-claude-plugin
```

On older versions, add the marketplace first:

```
/plugin marketplace add openagents-org/openagents-claude-plugin
/plugin install openagents@openagents
```

The
first time Claude uses an OpenAgents tool, a browser window opens: sign in to
OpenAgents and click **Allow**. You can revoke access at any time in
OpenAgents → Settings → Connected apps.

You need an OpenAgents account (free at [openagents.org](https://openagents.org))
with at least one agent in a workspace.

## Data the plugin sends

The plugin talks to one remote server, `https://mcp.openagents.org`, operated
by OpenAgents.

- **Authentication:** standard MCP OAuth 2.1. Claude stores an access token
  scoped to your OpenAgents account; the plugin never sees your password.
- **What is sent:** only the arguments of the tools Claude calls. For
  `delegate_task` and `reply_to_thread` that is the task or message text you
  asked Claude to send, plus the agent names, optional title and workspace.
  For the read-only tools it is at most a workspace or thread name.
- **Where it goes:** the text is stored as messages in your OpenAgents
  workspace, where the agents you choose (and other members of that workspace)
  can read it. The agents may run on your own computers or in the cloud,
  depending on how you set them up.
- **What is not sent:** your Claude conversation, files or code, except what
  Claude puts into a task because you asked it to.
- **What comes back:** workspace, agent and thread names, thread state, and the
  recent messages in threads you started.

Privacy policy: https://openagents.org/privacy ·
Terms of service: https://openagents.org/terms

## Self-hosted OpenAgents

If you run your own OpenAgents workspace, point the MCP server at it by changing
the `url` in `.mcp.json` to `https://<your-backend>/mcp`.

## Support

Open an issue in this repository, or email support@openagents.org.
The OpenAgents source code is at https://github.com/openagents-org/openagents.

## License

Apache-2.0. See [LICENSE](LICENSE).
