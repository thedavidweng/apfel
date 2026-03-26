# TICKET-011: Semantic Exit Codes

**Status:** Open
**Priority:** P3 (UNIX convention)
**Blocked by:** Nothing

---

## Goal

Return specific exit codes for different failure modes so shell scripts can
react to specific errors.

## Current

- 0 = success
- 1 = runtime error (catch-all)
- 2 = usage error (bad flags)

## Proposed

- 0 = success
- 1 = unknown runtime error
- 2 = usage error (bad flags, missing prompt)
- 3 = guardrail blocked (content policy violation)
- 4 = context overflow (input too long)
- 5 = model unavailable (Apple Intelligence not enabled/ready)
- 6 = rate limited / busy

## Usage in scripts

```bash
apfel "translate this" 2>/dev/null
case $? in
  0) echo "ok" ;;
  3) echo "blocked by guardrail, rephrasing..." ;;
  5) echo "model not ready, waiting..." ;;
esac
```

## Implementation

In `CLI.swift` and `main.swift`, catch errors, classify via `ApfelError.classify()`,
and map to the specific exit code. Add constants in `main.swift`.

## Files

- `Sources/main.swift` — exit code constants + error-to-exit mapping
- `Sources/CLI.swift` — use specific exit codes in singlePrompt/chat
