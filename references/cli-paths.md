# CLI Paths

### Pattern: powershell-quoted-cli-paths
- scenario: External CLI invocation with absolute Windows paths
- use_when: The task needs a Windows CLI and any input, output, or working path may contain spaces, non-ASCII characters, or deep directories.
- shell: PowerShell
- validated_shape: `<TOOL> <FLAGS> "<INPUT_PATH>" <MORE_FLAGS> "<OUTPUT_PATH>"`
- substitute_only: `<TOOL>`, `<FLAGS>`, `<INPUT_PATH>`, `<MORE_FLAGS>`, `<OUTPUT_PATH>`
- preflight: `Get-Command "<TOOL>"`; `Test-Path "<INPUT_PATH>"`; verify that the parent directory of `<OUTPUT_PATH>` exists or will be created explicitly
- env: none
- avoid: Unquoted paths; mixed slash styles copied blindly from prior commands; relying on the current working directory when absolute paths are known
- success_signal: The CLI runs without path parsing errors and reads or writes the intended files
- capture_rule: Update this entry when a safer quoting or preflight shape proves stable across multiple tasks
