# Before And After Command Repairs

These examples show the kind of repair this skill is meant to trigger after a first failed attempt.

## 1. Unquoted Paths

Before:

```powershell
pandoc D:\Work Files\draft.md -o D:\Output Files\draft.docx
```

After:

```powershell
Get-Command "pandoc"
Test-Path "D:\Work Files\draft.md"
pandoc -f markdown -t docx "D:\Work Files\draft.md" -o "D:\Output Files\draft.docx"
```

Why:

- quoted paths remove parsing ambiguity
- `Get-Command` checks that the tool actually exists

## 2. Bare Inline Python On Windows

Before:

```powershell
python -c "print('中文输出')"
```

After:

```powershell
$env:PYTHONIOENCODING='utf-8'
$env:PYTHONUTF8='1'
python -c "print('中文输出')"
```

Why:

- the encoding becomes explicit
- the output is less likely to break in default Windows environments

## 3. Repeating The Same Failed Search Shape

Before:

```powershell
rg TODO D:\Projects\demo
```

After:

```powershell
Test-Path "D:\Projects\demo"
Get-ChildItem "D:\Projects\demo" -Recurse -File | Select-String -Pattern "TODO"
```

Why:

- it stops assuming `rg` is installed and usable
- it switches to a PowerShell-native fallback

## 4. Archive Extraction Without Exact Path Handling

Before:

```powershell
Expand-Archive D:\Downloads\release.zip D:\Extracted\release
```

After:

```powershell
Test-Path "D:\Downloads\release.zip"
Expand-Archive -LiteralPath "D:\Downloads\release.zip" -DestinationPath "D:\Extracted\release" -Force
```

Why:

- `-LiteralPath` is safer when exact path handling matters
- the input path is verified before extraction
