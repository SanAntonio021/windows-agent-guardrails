# Archive And File Ops

### Pattern: powershell-archive-and-file-ops
- scenario: Archive extraction and native file operations
- use_when: The task needs to copy, move, remove, or extract files on Windows and a native PowerShell command exists
- shell: PowerShell
- validated_shape: `<NATIVE_CMD> -LiteralPath "<INPUT_PATH>" <DEST_FLAGS> "<OUTPUT_PATH>" <EXTRA_FLAGS>`
- substitute_only: `<NATIVE_CMD>`, `<INPUT_PATH>`, `<DEST_FLAGS>`, `<OUTPUT_PATH>`, `<EXTRA_FLAGS>`
- preflight: `Test-Path "<INPUT_PATH>"`; if extracting, verify the destination parent path; prefer `-LiteralPath` for archive and exact-path operations
- env: none
- avoid: Shelling out to another archive tool without need; using text-editing commands for file movement; destructive removal without first verifying the target
- success_signal: The intended files are copied, moved, removed, or extracted without path interpretation issues
- capture_rule: Update this entry when a native PowerShell file-operation shape proves safer than an external CLI alternative
