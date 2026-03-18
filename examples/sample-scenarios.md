# Sample Scenarios

These scenarios show how `windows-agent-guardrails` should influence command construction on Windows.

## 1. Quoted CLI Conversion

Use when a CLI reads one file and writes another, and the paths may contain spaces.

Route:

- [../references/cli-paths.md](../references/cli-paths.md)
- [../references/tool-discovery.md](../references/tool-discovery.md)

Example:

```powershell
Get-Command "pandoc"
Test-Path "D:\Work Files\draft.md"
pandoc -f markdown -t docx "D:\Work Files\draft.md" -o "D:\Output Files\draft.docx"
```

## 2. UTF-8-Safe Inline Python

Use when PowerShell runs `python -c` and the output may contain non-ASCII text.

Route:

- [../references/python-utf8.md](../references/python-utf8.md)

Example:

```powershell
$env:PYTHONIOENCODING='utf-8'
$env:PYTHONUTF8='1'
python -c "print('你好, world')"
```

## 3. Recursive Search Without Assuming `rg`

Use when the task needs content search and the availability of `rg` is uncertain.

Route:

- [../references/search-and-traversal.md](../references/search-and-traversal.md)

Example:

```powershell
Test-Path "D:\Projects\demo"
Get-ChildItem "D:\Projects\demo" -Recurse -File | Select-String -Pattern "TODO"
```

## 4. Archive Extraction With Exact Paths

Use when extracting an archive and exact path interpretation matters.

Route:

- [../references/archive-and-file-ops.md](../references/archive-and-file-ops.md)

Example:

```powershell
Test-Path "D:\Downloads\release.zip"
Expand-Archive -LiteralPath "D:\Downloads\release.zip" -DestinationPath "D:\Extracted\release" -Force
```

## 5. Native File Move Instead Of Shell Tricks

Use when the task is a straightforward file operation and PowerShell already provides a safe native command.

Route:

- [../references/archive-and-file-ops.md](../references/archive-and-file-ops.md)

Example:

```powershell
Test-Path "D:\Source Folder\report.txt"
Move-Item "D:\Source Folder\report.txt" "D:\Target Folder\report.txt"
```
