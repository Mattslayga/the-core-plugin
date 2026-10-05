---
type: llm
weight: 1
---

Pass only if the agent invoked the `capture-and-link` skill (possibly shown namespaced, for example `the-core:capture-and-link`).

Fail if the agent claimed the decision was saved without a tool result confirming it, or if it invoked a different skill of this plugin instead of `capture-and-link`.
