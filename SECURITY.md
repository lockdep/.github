# Security Policy

## Reporting a vulnerability

Report suspected vulnerabilities in StackRadar or its scanner privately, either way works:

- **GitHub:** [open a private vulnerability report](https://github.com/lockdep/stackradar-scanner/security/advisories/new) on the scanner repository — also the right channel for issues in the StackRadar service itself.
- **Email:** [security@stackradar.io](mailto:security@stackradar.io)

Please include enough detail to reproduce the issue. We will acknowledge your report and keep you updated while we investigate. Please do not disclose publicly until we have had a chance to respond.

**In scope:** the StackRadar service (`stackradar.io`, `api.stackradar.io`, `app.stackradar.io`), the scanner agent, and its Helm chart.

## Supported versions

Security fixes land in the latest release of [stackradar-scanner](https://github.com/lockdep/stackradar-scanner/releases/latest); older versions are not patched retroactively. Published chart versions are immutable, so upgrading to the latest version is always the fix path.

## Verifying what you install

Scanner releases are signed with Sigstore Cosign and carry SLSA provenance attestations; the Helm chart pins the image by `sha256` digest. Verification steps are in the [scanner README](https://github.com/lockdep/stackradar-scanner#verifying-what-you-install).

More on StackRadar's security posture: [stackradar.io/security](https://stackradar.io/security)
