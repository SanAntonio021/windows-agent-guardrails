# Python UTF-8

### Pattern: powershell-python-utf8-inline
- scenario: PowerShell plus inline Python with Unicode-safe output
- use_when: A command uses `python -c` or similar inline Python in PowerShell and the task may emit non-ASCII output
- shell: PowerShell
- validated_shape: `$env:PYTHONIOENCODING='utf-8'; $env:PYTHONUTF8='1'; python -c "<PYTHON_CODE>"`
- substitute_only: `<PYTHON_CODE>`
- preflight: `Get-Command "python"`; keep the Python code compact; switch to a script file if quoting becomes fragile
- env: `PYTHONIOENCODING=utf-8`; `PYTHONUTF8=1`
- avoid: Bare `python -c` with default Windows encoding; large multiline code jammed into one inline command without checking quoting
- success_signal: Python runs without encoding errors and emits the expected UTF-8 text
- capture_rule: Update this entry when a more reliable inline quoting or UTF-8 setup becomes stable
