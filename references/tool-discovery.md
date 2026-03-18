# Tool Discovery

### Pattern: powershell-tool-discovery-then-run
- scenario: Windows external tool execution with explicit discovery
- use_when: The task depends on a non-builtin tool such as `pandoc`, `ffmpeg`, or another CLI that may be missing, shadowed, or version-dependent
- shell: PowerShell
- validated_shape: `Get-Command "<TOOL>"; <TOOL> <FLAGS> "<INPUT_PATH>" <MORE_FLAGS>`
- substitute_only: `<TOOL>`, `<FLAGS>`, `<INPUT_PATH>`, `<MORE_FLAGS>`
- preflight: `Get-Command "<TOOL>"`; `Test-Path "<INPUT_PATH>"`; if the tool writes files, verify output directory handling explicitly
- env: none
- avoid: Assuming the tool is already on PATH; retrying the same failing invocation without checking tool availability first
- success_signal: The tool is discoverable and the command completes without executable-not-found or quoting errors
- capture_rule: Update this entry when a tool-specific preflight becomes stable enough to reuse beyond a single project
