# Pastewatch Action

GitHub Action for scanning files for sensitive data patterns using [pastewatch-cli](https://github.com/ppiankov/pastewatch).

Deterministic regex-based detection. No ML. No network calls.

## Usage

```yaml
- uses: ppiankov/pastewatch-action@v1
  with:
    path: ./src
```

### With SARIF upload (GitHub code scanning)

```yaml
- uses: ppiankov/pastewatch-action@v1
  with:
    path: .
    upload-sarif: true
```

### With baseline (suppress known findings)

```yaml
- uses: ppiankov/pastewatch-action@v1
  with:
    path: .
    baseline: .pastewatch-baseline.json
```

### With allowlist and custom rules

```yaml
- uses: ppiankov/pastewatch-action@v1
  with:
    path: .
    allowlist: .pastewatch-allow
    rules: custom-rules.json
```

### Full example

```yaml
name: Security Scan
on: [push, pull_request]

jobs:
  pastewatch:
    runs-on: macos-14
    steps:
      - uses: actions/checkout@v4
      - uses: ppiankov/pastewatch-action@v1
        with:
          path: ./src
          format: sarif
          upload-sarif: true
          baseline: .pastewatch-baseline.json
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `path` | Directory to scan | `.` |
| `version` | pastewatch-cli version | `0.4.0` |
| `format` | Output format: text, json, sarif | `text` |
| `check` | Check mode (exit code only) | `true` |
| `allowlist` | Path to allowlist file | |
| `rules` | Path to custom rules JSON | |
| `baseline` | Path to baseline file | |
| `upload-sarif` | Upload SARIF to code scanning | `false` |

## Outputs

| Output | Description |
|--------|-------------|
| `findings-count` | Number of findings detected |
| `report-file` | Path to the report file |

## Requirements

- **macOS runner required** (`runs-on: macos-14`). pastewatch-cli is a native macOS ARM binary.
- For SARIF upload, the repository needs GitHub Advanced Security enabled (free for public repos).

## Exit Codes

The action fails (exit 1) when findings are detected. Use `continue-on-error: true` if you want findings to be non-blocking.

## License

MIT
