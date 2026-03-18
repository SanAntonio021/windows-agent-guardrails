# Search And Traversal

### Pattern: powershell-search-and-traversal
- scenario: File search and content search on Windows
- use_when: The task needs to search by file name, traverse directories, or inspect text content from PowerShell
- shell: PowerShell
- validated_shape: `Get-ChildItem "<ROOT_PATH>" -Recurse <FILTERS> | Select-String -Pattern "<PATTERN>"`
- substitute_only: `<ROOT_PATH>`, `<FILTERS>`, `<PATTERN>`
- preflight: `Test-Path "<ROOT_PATH>"`; confirm whether file-name search or content search is required before adding `Select-String`
- env: none
- avoid: Reading binary files as text; assuming `rg` is usable without checking; omitting quotes around `<ROOT_PATH>`
- success_signal: The command returns only the intended files or matching lines without shell syntax errors
- capture_rule: Update this entry when a clearer split between traversal and content search proves stable across tasks
