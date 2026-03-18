# Pattern Library Maintenance

Use this file to decide when a command shape should enter the reusable pattern library.

## Entry Template

```md
### Pattern: <short-id>
- scenario: <high-level command family>
- use_when: <when this pattern should be preferred>
- shell: <PowerShell / other>
- validated_shape: <command skeleton with placeholders>
- substitute_only: <which placeholders may be replaced at runtime>
- preflight: <checks to run before execution>
- env: <environment variables or `none`>
- avoid: <shapes that should not be reused>
- success_signal: <what counted as validated>
- capture_rule: <when to update this entry instead of adding a new one>
```

## Write Rules

- Store reusable command skeletons, not real user paths, usernames, or private filenames.
- Prefer updating the closest existing entry over adding a near-duplicate entry.
- Do not store failed commands as validated patterns.
- Use placeholders such as `<TOOL>`, `<INPUT_PATH>`, and `<OUTPUT_PATH>`.

## High-Value Capture

Add or update a pattern only when at least one of these is true:

- a similar command shape has failed before
- the scenario is high-risk because of quoting, encoding, paths, or external CLIs
- the repaired command is likely to be reusable across tasks

Do not capture:

- one-off project-specific commands
- commands that worked only because of an accidental working directory
- commands that still contain private context
- unstable trial-and-error commands
