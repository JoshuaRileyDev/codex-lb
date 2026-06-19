## MODIFIED Requirements
### Requirement: Map chat requests to Responses wire format
The service MUST map chat messages into the Responses request format by merging `system`/`developer` content into `instructions` and forwarding all other messages as `input`. Tool definitions MUST be normalized to the Responses tool schema, and `tool_choice`, `reasoning_effort`, and `response_format` MUST be mapped consistently. Unsupported fields MUST not be silently ignored if they change behavior.

Function-style tool definitions with a nested `function` object MUST be normalized to standard `function` tools even when the incoming `type` is a custom app-level action name. This includes Codex thread actions such as `create_thread`, `fork_thread`, `list_threads`, `read_thread`, `send_message_to_thread`, `handoff_thread`, `set_thread_pinned`, `set_thread_archived`, and `set_thread_title`.

#### Scenario: System message normalization
- **WHEN** the client sends a `system` message followed by a `user` message
- **THEN** the service maps the system content to `instructions` and the user message to `input`

#### Scenario: Tool choice values
- **WHEN** the client sets `tool_choice` to `none`, `auto`, or `required`
- **THEN** the service forwards the value consistently in the mapped Responses request

#### Scenario: Custom thread action tool is normalized as a function tool
- **WHEN** the client sends a tool with `type: "create_thread"` and a nested `function` definition
- **THEN** the mapped Responses request includes a standard `function` tool named `create_thread`

#### Scenario: Custom thread action tool choice is normalized as a function choice
- **WHEN** the client sends `tool_choice` with `type: "create_thread"` and a nested `function` name of `create_thread`
- **THEN** the mapped Responses request includes `tool_choice: {"type":"function","name":"create_thread"}`

