# Model routing evidence

## Provenance and interpretation

The project owner supplied these shared-benchmark averages during requirements definition on 2026-10-04. The two tables cover different benchmark sets; compare configurations within the same table. Raw benchmark results, individual weights, and measurement methodology were not supplied, so these aggregates establish a starting point for routing that must be checked on representative tasks.

The owner defines `Cost` as a weighted indicator for comparing fees across models. Its formula, units, normalization, and scoring direction are unspecified. Preserve the reported values as comparison evidence; actual task usage also depends on context, reasoning, tool calls, retries, verification, and coordination. This skill uses task fit to select a configuration and does not calculate a numerical cost-performance ratio from this column.

## Shared 30-benchmark reference

| Model | Reasoning Effort | Overall | Agentic | Coding | Reasoning | Knowledge | Comprehension | Language |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| GPT-6 Astra | max | 65.5 | 45.1 | 68.8 | 80.7 | 60.3 | 55.6 | 82.5 |
| GPT-6.1 Sol | max | 64.4 | 44.6 | 66.3 | 78.6 | 59.4 | 55.6 | 82.1 |
| GPT-6 Luna | max | 52.9 | 37.2 | 59.9 | 61.8 | 42.1 | 51.2 | 64.9 |

## Shared 11-benchmark reference

| Model | Reasoning Effort | Overall | Agentic | Coding | Reasoning | Knowledge | Comprehension | Cost |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| GPT-6 Astra | max | 54.9 | 48.5 | 61.0 | 63.4 | 58.6 | 31.0 | 81.7 |
| GPT-6.1 Sol | max | 53.9 | 48.5 | 57.9 | 62.9 | 57.5 | 31.0 | 59.5 |
| GPT-6.1 Sol | high | 52.5 | 43.9 | 59.7 | 60.8 | 56.1 | 32.0 | 51.2 |
| GPT-6.1 Sol | medium | 50.4 | 40.8 | 58.8 | 57.2 | 55.1 | 30.0 | 46.9 |
| GPT-6 Luna | max | 40.6 | 36.3 | 54.5 | 39.4 | 41.1 | 22.8 | 32.1 |

## Policy derived for this project

These are project routing decisions to evaluate in practice:

- Sol `medium` is the general default for clearly specified, multi-step delegated work.
- Luna `max` handles bounded work with straightforward acceptance checks. In the 11-benchmark set, its Coding score is closer to Sol `medium` than its Reasoning score, supporting careful scoping and verification.
- Sol `high` handles work with greater ambiguity, interacting assumptions, or deeper verification needs. Its Overall score exceeds Sol `medium` by 2.1 points in the 11-benchmark set; the differences vary by domain.
- Astra is the intended coordinator profile. The user chooses the actual parent configuration; the Astra `max` benchmark row does not prescribe the parent's reasoning effort.

The `max` rows for Astra and Sol provide model-family context. The three worker configurations in `SKILL.md` define the available routes. These averages do not establish task-specific success rates or completion times.

## Official guidance

Official documentation provides the platform behavior and general model guidance. The benchmark tables above come from the project owner.

- [Model guidance: subagent delegation](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-6-astra#subagent-delegation): prompt for useful parallel delegation and legible inter-agent messages.
- [Model guidance: instruction following](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-6-astra#instruction-following): make user-instruction priority explicit and identify skill instructions that cause a pause or change of direction.
- [Subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents): project or skill instructions can trigger Codex delegation; model and reasoning settings can be selected for individual agents.
- [GPT-6 model guidance](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-6-astra): model positioning and reasoning configuration guidance.

The selected worker combinations are project choices. API parameter names and native Codex tool fields differ; use the schema actually exposed by the client.
