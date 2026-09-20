---
title: Cross-Branch Merge Safety Check
description: Before merging or pushing, check whether your branch is compatible with other active branches. Use the Rosentic MCP tools: check_conflicts to scan for breaks, explain_conflict for details, list_branches to see what's in flight.
---

# Cross-Branch Merge Safety Check

Use this skill when the user is about to push, open a PR, merge, rebase, or make a contract-affecting change such as changing a function signature, HTTP route, GraphQL schema, OpenAPI spec, or protobuf message.

Rosentic checks integration-state compatibility across active branches. It is not a code review tool and does not judge style, maintainability, or general code quality.

## Workflow

1. Use `list_branches` to see active branches in the repository.
2. Use `check_conflicts` to scan for cross-branch contract conflicts.
3. If findings are returned, use `explain_conflict` on the highest-priority finding before recommending next steps.
4. If no findings are returned, report that Rosentic did not detect cross-branch contract conflicts in the scanned branch set.

## Tool Examples

List active branches:

```json
{
  "tool": "list_branches",
  "arguments": {
    "repo_path": "."
  }
}
```

Scan the current repository:

```json
{
  "tool": "check_conflicts",
  "arguments": {
    "repo_path": ".",
    "base": "main",
    "format": "summary"
  }
}
```

Scan a specific branch:

```json
{
  "tool": "check_conflicts",
  "arguments": {
    "repo_path": ".",
    "branch": "feature/change-api",
    "base": "main",
    "format": "json"
  }
}
```

Explain a finding:

```json
{
  "tool": "explain_conflict",
  "arguments": {
    "repo_path": ".",
    "finding_id": "finding-id-from-check-conflicts"
  }
}
```

## Response Guidance

Keep the response focused on merge safety:

- State the number of unsafe findings and warnings.
- Name the affected branches and changed symbol or contract.
- Recommend which branch should update its caller, route consumer, or schema usage.
- Do not describe Rosentic findings as AI review findings.
- Do not claim code is broken unless Rosentic returned an `UNSAFE` finding with evidence.

