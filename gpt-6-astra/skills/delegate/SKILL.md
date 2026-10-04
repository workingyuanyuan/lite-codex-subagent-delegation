---
name: delegate
description: Delegate work to Codex subagents when it improves efficiency or quality.
---

# Delegate

Meet the required quality, then balance total usage and completion time.

The user's instructions take precedence over this skill's guidelines. If this skill causes confirmation, a pause, incomplete work, or divergence from the user's intent, link the exact `SKILL.md`, quote the relevant instruction, and explain its application, distinguishing requirements from interpretation.

## Select a configuration

Choose among these configurations by task requirements:

| Model ID | Reasoning effort | Task fit |
| --- | --- | --- |
| `gpt-6-luna` | `max` | Narrow, explicit, readily checked extraction, search, organization, or local edits. |
| `gpt-6.1-sol` | `medium` | Default for general delegated work: multi-step implementation, tests, and cross-file investigation. |
| `gpt-6.1-sol` | `high` | Complex debugging, interacting dependencies, uncertain assumptions, or deep review. |

Read [benchmark evidence](references/benchmarks.md) when tuning this policy.

## Delegate and integrate

Use native collaboration tools and explicitly set the selected model and reasoning effort. For `collaboration.spawn_agent`, use `model` and `reasoning_effort`, with `fork_turns: "none"` by default; full-history forks inherit the parent's configuration and reject these overrides.

If a configuration is unavailable, select another suitable configuration from the table or handle the work yourself; if delegation is unavailable, handle it yourself.

Give each child its objective, necessary context and constraints, and completion criteria. Keep messages legible, with proper spacing between words and numbers.

Assign disjoint write scopes for parallel work in a shared workspace. The coordinator owns final verification and integration.

For insufficient results, address the specific failure with clarification, a better-suited configuration, or coordinator takeover.
