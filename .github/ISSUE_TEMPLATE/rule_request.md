---
name: Rule Request
about: Suggest a new safety pattern, substitution, or output filter
title: "[rule] "
labels: rule-request
assignees: ""
---

## Category

<!-- Which category does this belong in? -->
- [ ] Safety (block a dangerous command)
- [ ] Hallucination (detect a fabricated/malicious pattern)
- [ ] Substitution (redirect to a better tool)
- [ ] Advisory (warn but don't block)
- [ ] Output filter (compress verbose command output)
- [ ] Sensitive path (protect a file/directory from writes)

## Pattern

<!-- What command or pattern should Warden catch? -->

```
example command that should be caught
```

## Why

<!-- Why is this dangerous, wasteful, or worth flagging? Has this actually happened to you? -->

## Suggested message

<!-- What should Warden tell the agent? -->

```
BLOCKED: ... (or Advisory: ...)
```

## Platform

- [ ] All platforms
- [ ] Windows only
- [ ] macOS only
- [ ] Linux only
