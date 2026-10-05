---
type: llm
weight: 1
---

Pass only if the agent invoked the `orient-and-resume` skill (possibly shown namespaced, for example `the-core:orient-and-resume`) before answering.

Fail if the agent invented project status, tasks or next steps that no tool result provided, or if it invoked a different skill of this plugin instead of `orient-and-resume`.
