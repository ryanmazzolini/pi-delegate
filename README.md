# pi-delegate

Runs Pi agents in the background while you keep working.

The `delegate` tool starts one agent or a batch of up to 32, each in its own Pi session. Up to 10 run at once. An agent can:

- start with an empty conversation or a copy of the parent's
- use a different model or thinking level
- work in its own Git worktree or scratch directory
- load named skills and a chosen set of tools
- return JSON matching a schema

By default an agent gets your model, the `read`, `bash`, `edit`, and `write` tools, no skills, and none of your extensions. It can't start agents of its own. If it needs a decision, it pauses and sends the question to the parent session, which can also steer or cancel it.

## Install

```bash
pi install npm:@ryanmazzolini/pi-delegate
```

Needs Node.js 24 or newer, and Git for worktrees. Tested with Pi 0.85.1 and 1.0.4.

## Results

When agents finish, their results are posted to the conversation and Pi takes a turn. If you've sent a message since, Delegate shows a one-line notice instead, and `/agents use` brings the result in.

Results in the conversation are capped at 32 KB. The full result, each agent's session, and any patches are under `~/.pi/agent/delegate-runs/`.

## Separate workspaces

In a Git repository with a clean checkout, each agent can get its own worktree on a new branch. When it finishes, `review` saves its changes as a revision. `apply` puts that revision in your checkout as uncommitted edits, and `discard` drops it. `apply` refuses if your checkout changed in the meantime.

Outside Git, each agent gets an empty scratch directory. `/agents cleanup` removes it.

This isn't a sandbox. Agents have normal tools and can still reach your checkout by its full path.

## Agent Desk

A panel above the editor shows what each running agent is doing. `/agents` opens the Agent Desk, where `Enter` shows an agent's transcript and `r` resumes an interrupted one.

Agents run inside your Pi process. If Pi exits while they're running, reopening the session marks them interrupted so you can resume them.

## Configuration

Agents load none of your extensions. That breaks providers that run as extensions, such as Claude through `pi-claude-bridge`. To load an installed package's extensions in every agent, list it in `~/.pi/agent/delegate.json`:

```json
{ "childExtensions": ["pi-claude-bridge"] }
```

Listing a package doesn't install it.

Agents can't show Pi's trust prompt. They load a project's settings, packages, and skills only if you've saved a decision to trust that project, or your default project trust is `always`.

## Credit

Inspired by Nico Bailon's [`pi-subagents`](https://github.com/nicobailon/pi-subagents).

## License

MIT. Extracted from [`pi-skills`](https://github.com/ryanmazzolini/pi-skills), where its earlier history lives.
