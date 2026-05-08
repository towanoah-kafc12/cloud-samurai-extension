# Common Bugs

## Entries

| ID | Title | Status | Scope | Detected On | Owner | Related Docs | Resolution |
| --- | --- | --- | --- | --- | --- | --- | --- |
| BUG-COM-001 | scripts に実行権限がない | Resolved | Common | 2026-05-08 | TBD | `scripts/project-build.sh`, `scripts/bootstrap.sh`, `scripts/lint.sh`, `scripts/test.sh` | `chmod +x` を付与し、README の Quick Start 形式で正常終了 |
