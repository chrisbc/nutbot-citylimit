# Agent Instructions

This file provides context for AI assistants (Claude, OpenCode, Cursor, etc.) working on this project.

## LLM Session Logging

When working on this project, **always append a log entry** to `.llm/log.jsonl` at the end of each session.

### Log Schema

```json
{
  "timestamp": "2025-02-22T10:30:00Z",
  "model": "claude-3-5-sonnet",
  "tool": "claude-code | opencode | cursor | other",
  "duration_min": 25,
  "tokens_in": 15000,
  "tokens_out": 8000,
  "commits": ["e263c5e", "abc1234"],
  "files": {
    "read": ["PLAN.md", "TRACTOR.md"],
    "created": ["README.md", "tractor/MOTOR_CONTROLLER.md"],
    "modified": ["README.md"]
  },
  "summary": "Brief description of work done",
  "user_notes": "Optional notes from user"
}
```

### Field Definitions

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `timestamp` | ISO 8601 string | Yes | Session start time (UTC) |
| `model` | string | Yes | Model identifier |
| `tool` | string | Yes | Tool name (claude-code, opencode, cursor, etc.) |
| `duration_min` | number | Yes | Approximate session duration in minutes |
| `tokens_in` | number | No | Input tokens used |
| `tokens_out` | number | No | Output tokens generated |
| `commits` | string[] | No | Git commit hashes made during session |
| `files` | object | No | File operations categorized |
| `files.read` | string[] | No | Files read/referenced |
| `files.created` | string[] | No | New files created |
| `files.modified` | string[] | No | Existing files modified |
| `summary` | string | Yes | Brief description of session work |
| `user_notes` | string | No | Optional user-provided notes |

### Implementation Notes

- **Append only**: Each session adds one line, never modify existing entries
- **JSONL format**: One valid JSON object per line (no trailing commas, no array wrapper)
- **File path**: Always `.llm/log.jsonl` in project root
- **Create if missing**: If `.llm/log.jsonl` doesn't exist, create it with the first entry

### Example Log Entry

```json
{"timestamp":"2025-02-22T10:30:00Z","model":"claude-3-5-sonnet","tool":"opencode","duration_min":25,"tokens_in":15000,"tokens_out":8000,"commits":["e263c5e"],"files":{"read":["PLAN.md","TRACTOR.md","IMPLEMENT_NUT_COLLECTOR.md","FIELD_OBSERVATIONS.md","DESIGN_VISUALS.md"],"created":["README.md","tractor/MOTOR_CONTROLLER.md","tractor/SENSORS_AND_COMPUTE.md","tractor/RC_ESC_ANALYSIS.md","tractor/OEM_CONTROLLER_ANALYSIS.md"],"modified":[]},"summary":"Add project README, motor controller research, sensors/compute docs, ROS2 references","user_notes":""}
```

## Project Context

- **Project**: NutBot CityLimit - autonomous acorn collection robot
- **Platform**: Recycled electric wheelchair chassis, 2WD differential drive
- **Code**: Currently documentation/planning phase, no code yet
- **Key docs**: README.md, PLAN.md, tractor/ directory