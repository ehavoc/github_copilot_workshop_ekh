# Project Plan: Task Manager CLI

## 1. Project overview
The Task Manager CLI is a small Node.js 20+ command-line application for managing personal tasks entirely in memory during runtime. Users can create, list, update, and delete tasks, then narrow results with filtering and sorting to focus on what matters most. The tool is intentionally lightweight for workshop use, using only built-in Node.js modules and a clear command structure that is easy to extend.

## 2. User stories
1. As a user, I want to create a task so I can track work I need to do.
- Acceptance criteria:
  - The CLI provides a `create` command with required `title` and optional `description`.
  - New tasks default to `status=todo` and `priority=medium` unless explicitly provided.
  - Each created task receives `createdAt` and `updatedAt` timestamps.
  - The CLI returns the created task with its generated ID.

2. As a user, I want to list all tasks so I can see my current workload.
- Acceptance criteria:
  - The CLI provides a `list` command.
  - The command prints all tasks in a readable table-like output.
  - If no tasks exist, the CLI shows a friendly empty-state message.

3. As a user, I want to update task details so I can reflect progress and changes.
- Acceptance criteria:
  - The CLI provides an `update` command that accepts task ID and fields to modify.
  - Allowed updates include `title`, `description`, `status`, and `priority`.
  - Invalid `status` or `priority` values are rejected with clear validation errors.
  - `updatedAt` changes on every successful update, while `createdAt` remains unchanged.

4. As a user, I want to delete a task so I can remove completed or irrelevant items.
- Acceptance criteria:
  - The CLI provides a `delete` command with task ID.
  - Deleting a valid ID removes the task from memory.
  - Deleting a non-existent ID returns a clear not-found message.

5. As a user, I want to filter tasks by status so I can focus on a specific workflow stage.
- Acceptance criteria:
  - The `list` command supports `--status` with values `todo`, `in-progress`, `done`.
  - The output contains only tasks matching the provided status.
  - Invalid filter values produce a validation error.

6. As a user, I want to filter tasks by priority so I can focus on urgency.
- Acceptance criteria:
  - The `list` command supports `--priority` with values `low`, `medium`, `high`.
  - The output contains only tasks matching the provided priority.
  - Invalid filter values produce a validation error.

7. As a user, I want to sort tasks so I can view them in a useful order.
- Acceptance criteria:
  - The `list` command supports `--sort` with `priority` and `createdAt`.
  - Priority sorting follows explicit order: `high`, `medium`, `low`.
  - Date sorting orders by `createdAt` ascending by default.
  - Optional `--desc` reverses the selected sort order.

8. As a user, I want clear command help so I can use the CLI without reading source code.
- Acceptance criteria:
  - Running with `--help` or no command shows usage and available options.
  - Each command has examples and required/optional argument notes.

## 3. Data model
- Task
  - `id: string` (unique identifier, e.g., incremental string or `crypto.randomUUID()`)
  - `title: string` (required, non-empty)
  - `description: string` (optional, default `""`)
  - `status: "todo" | "in-progress" | "done"` (default `"todo"`)
  - `priority: "low" | "medium" | "high"` (default `"medium"`)
  - `createdAt: string` (ISO 8601 timestamp)
  - `updatedAt: string` (ISO 8601 timestamp)

- In-memory store
  - `tasks: Task[]`
  - Behavior: lives for process lifetime only; resets when CLI exits.

## 4. File structure
```text
src/
  index.js                # CLI entry point
  cli/
    parser.js             # Argument parsing and command routing
    help.js               # Usage text and examples
  models/
    task.js               # Task schema helpers and defaults
  services/
    taskService.js        # CRUD operations, filter/sort logic
  store/
    memoryStore.js        # In-memory task array and store accessors
  utils/
    validators.js         # Validation for status, priority, input fields
    formatters.js         # Output formatting for terminal display
    constants.js          # Enums and sort precedence maps
```

## 5. Implementation phases
1. Phase 1: Project skeleton and CLI shell
- Create `src/` structure and entry file.
- Implement command parser skeleton (`create`, `list`, `update`, `delete`, `help`).
- Add usage/help output with examples.
- Milestone outcome: Commands are recognized and routed.

2. Phase 2: Core task model and in-memory store
- Define task shape, allowed status/priority values, and defaults.
- Implement in-memory array store and ID generation strategy.
- Add timestamp helpers for `createdAt` and `updatedAt`.
- Milestone outcome: Tasks can be represented and persisted in memory.

3. Phase 3: CRUD command implementation
- Implement create workflow with validation.
- Implement list workflow with basic output.
- Implement update and delete by ID with not-found handling.
- Milestone outcome: End-to-end CRUD works from CLI commands.

4. Phase 4: Filtering, sorting, and output polish
- Add `--status` and `--priority` filters to list.
- Add `--sort` and `--desc` sorting options.
- Improve output formatting for readability.
- Milestone outcome: Users can refine task views effectively.

5. Phase 5: Validation, edge cases, and tests
- Cover invalid enum values, missing required fields, and invalid IDs.
- Add Node.js built-in test runner tests (`node:test`, `assert`) for service logic.
- Smoke-test command flows manually.
- Milestone outcome: CLI behavior is reliable and workshop-ready.
