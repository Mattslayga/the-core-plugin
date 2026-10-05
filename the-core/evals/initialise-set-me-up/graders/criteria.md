---
type: llm
weight: 1
---

Pass only if the agent invoked the `initialise-context` skill (possibly shown namespaced, for example `the-core:initialise-context`).

Fail if the agent claimed any records were created, or wrote anything, before showing a preview and receiving explicit approval from the user. Fail if it invoked a different skill of this plugin instead of `initialise-context`.
