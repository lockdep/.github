# StackRadar

**Know which Kubernetes pods a new security flaw affects — the moment the CVE is published.**

StackRadar keeps an up-to-date dependency list — an SBOM — for every container image running in your clusters. When a new CVE lands, it is matched against that inventory instantly: no new scan, no waiting.

## How it works

A small agent runs in your cluster, watches pods in real time, generates a CycloneDX SBOM per image with [Syft](https://github.com/anchore/syft), and uploads it to StackRadar. Findings are matched against a continuously synced mirror of [OSV.dev](https://osv.dev), so every finding can be checked against its public advisory independently of us.

The only thing the agent sends outward is the SBOM and the workload metadata to attribute it — never source code, image layers, secrets, or credentials. Details on [stackradar.io/security](https://stackradar.io/security).

## Install

```bash
helm install stackradar-scanner oci://ghcr.io/lockdep/charts/stackradar-scanner \
  --version <version> \
  --namespace stackradar --create-namespace
```

Always pin an explicit `--version` — published versions are immutable. The latest is on the [releases page](https://github.com/lockdep/stackradar-scanner/releases/latest).

## Audit what you install

The agent runs with cluster-wide read access, so its source is published for you to read: [**lockdep/stackradar-scanner**](https://github.com/lockdep/stackradar-scanner). Every release is signed with Sigstore Cosign, built in GitHub Actions with SLSA provenance attestations, and the chart pins the image by `sha256` digest.

---

[stackradar.io](https://stackradar.io) · [Docs](https://stackradar.io/docs) · [Security](https://stackradar.io/security) · [Report a vulnerability](https://github.com/lockdep/stackradar-scanner/security/advisories/new)
