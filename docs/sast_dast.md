# SAST and DAST Scans

This document describes the Static Application Security Testing (SAST) and Dynamic Application Security Testing (DAST) scan results for the RetailPulse project.

## Backend SAST Scan Results

The backend codebase was scanned using Semgrep to identify security vulnerabilities and code quality issues.

### Semgrep Security Scan Results

```out
┌─────────────┐
│ Scan Status │
└─────────────┘
  Scanning 54 files tracked by git with 1062 Code rules:

  Language      Rules   Files          Origin      Rules
 ─────────────────────────────        ───────────────────
  <multilang>      48      54          Community    1062
  python          243      46

┌────────────────┐
│ 1 Code Finding │
└────────────────┘

    /src/app/main.py
    ❯❱ python.fastapi.security.wildcard-cors.wildcard-cors
          CORS policy allows any origin (using wildcard '*'). This is insecure and should be avoided.
          Details: https://sg.run/KxApY

           20┆ allow_origins=["*"],

┌──────────────┐
│ Scan Summary │
└──────────────┘
✅ Scan completed successfully.
 • Findings: 1 (1 blocking)
 • Rules run: 291
 • Targets scanned: 54
 • Parsed lines: ~100.0%
 • Scan skipped:
   ◦ Files matching .semgrepignore patterns: 10
 • Scan was limited to files tracked by git
 • For a detailed list of skipped files and lines, run semgrep with the --verbose flag
Ran 291 rules on 54 files: 1 finding.
```

## Frontend SAST Scan Results

The frontend codebase was scanned using Semgrep to identify security vulnerabilities and code quality issues.

### Scan Report

```out
┌─────────────┐
│ Scan Status │
└─────────────┘
  Scanning 65 files tracked by git with 1062 Code rules:

  Language      Rules   Files          Origin      Rules
 ─────────────────────────────        ───────────────────
  <multilang>      61      65          Community    1062
  js              156      54
  json              4       2
  html              1       1

┌──────────────┐
│ Scan Summary │
└──────────────┘
✅ Scan completed successfully.
 • Findings: 0 (0 blocking)
 • Rules run: 221
 • Targets scanned: 65
 • Parsed lines: ~100.0%
 • Scan skipped:
   ◦ Files matching .semgrepignore patterns: 3
 • Scan was limited to files tracked by git
 • For a detailed list of skipped files and lines, run semgrep with the --verbose flag
Ran 221 rules on 65 files: 0 findings.
(need more rules? `semgrep login` for additional free Semgrep Registry rules)

If Semgrep missed a finding, please send us feedback to let us know!
See https://semgrep.dev/docs/reporting-false-negatives/
```

## Security Scan Summary

### Key Findings

- **Backend**: 1 security finding (CORS wildcard policy - blocking)
- **Frontend**: 0 security findings
- **Total Findings**: 1 blocking issue

## Running SAST Scans Locally

### Backend (Python)

```bash
docker run --rm -v "$(pwd)":/src returntocorp/semgrep \
  semgrep scan --config=auto /src > semgrep_results.txt
```

### Frontend (JavaScript/React)

```bash
docker run --rm -v "$(pwd)":/src returntocorp/semgrep \
  semgrep scan --config=auto /src > semgrep_results.txt
```
