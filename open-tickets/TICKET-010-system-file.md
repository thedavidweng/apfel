# TICKET-010: --system-file Flag

**Status:** Open
**Priority:** P3 (UNIX convention)
**Blocked by:** Nothing

---

## Goal

Read system prompt from a file instead of inline argument.

## Usage

```bash
apfel --system-file prompt.txt "What should I do?"
apfel --system-file ~/.apfel-system.txt --chat
cat prompt.txt | apfel --system-file - "question"  # stdin for system prompt
```

## Why

Complex system prompts (multi-paragraph, with examples) don't fit on a command line.
Real UNIX tools support file input. This is `--system` but from a file.

## Implementation

In `main.swift`, add `--system-file` flag. Read the file content and assign to `systemPrompt`.
Support `-` for stdin (but only if the main prompt is provided as an argument, not piped).

## Files

- `Sources/main.swift` — flag parsing
- `Sources/CLI.swift` — update usage text
