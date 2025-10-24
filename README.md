# Shift-Left Demo: Flask app with Semgrep + Trivy scans

This demo repository shows how to run **Semgrep** and **Trivy** scans in a GitHub Actions workflow to implement "shift-left" security checks.

## Contents
- `app.py` - simple Flask app
- `requirements.txt` - Python dependencies
- `Dockerfile` - builds a container image for scanning with Trivy
- `.github/workflows/security-scan.yml` - GitHub Actions workflow that runs Semgrep (static analysis) and Trivy (container + dependency scan)

## How it works (workflow)
1. On push or pull_request, the workflow:
   - Checks out the code
   - Runs Semgrep to find common security issues in source
   - Builds a Docker image
   - Runs Trivy to scan the image for vulnerabilities (and configuration issues)
2. The job is configured to **fail** the build if vulnerabilities with CVSS >= 7 are found.

## Local testing
- Run semgrep locally (install from pip): `pip install semgrep` and then `semgrep --config=r2c` or a ruleset of your choice.
- Build the image: `docker build -t shift-left-demo:latest .`
- Run trivy locally: `trivy image --exit-code 1 --severity CRITICAL,HIGH shift-left-demo:latest`

## Notes
- Adjust severity thresholds in the workflow to match your policy.
- Replace example semgrep rules with your organization's ruleset for better signal-to-noise.
