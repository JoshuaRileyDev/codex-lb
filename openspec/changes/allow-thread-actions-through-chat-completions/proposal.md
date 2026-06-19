# Proposal: Allow thread action tools through Chat Completions

## Summary
Normalize Codex app thread-management tool definitions so they are forwarded as standard function tools instead of being rejected or preserved with unsupported custom tool types.

## Why
The Codex app exposes thread actions such as `create_thread`, `fork_thread`, and `send_message_to_thread`. When those tool definitions reach the proxy, they need to survive request normalization and be emitted upstream in a format OpenAI Chat Completions accepts.

## Scope
- Normalize function-style tool definitions to `type: "function"` during Chat Completions mapping.
- Normalize function-style `tool_choice` objects the same way.
- Keep existing behavior for web search tool aliases and other validated request fields.

