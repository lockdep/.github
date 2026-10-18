# StackRadar

**Open-source Kubernetes vulnerability scanner: an SBOM of every running image, matched against OSV and ranked by CVSS, EPSS and CISA KEV.**

A scanner tells you your cluster has four thousand CVEs. StackRadar tells you which of them are on images that are running right now, which of those are being exploited in the wild, and which chart version or image tag makes them go away. When a new advisory is published, the dashboard already knows which pods run it, without pulling or scanning anything again.

Self-host it under AGPL-3.0 with one `helm install`, or use the hosted version at [app.stackradar.io](https://app.stackradar.io). [Live demo](https://demo.stackradar.io/demo/enter).

## Repositories

| Repository | What it is | Licence |
|:--|:--|:--|
| [**stackradar**](https://github.com/lockdep/stackradar) | The server, the web client and the self-hosting docs. Start here. | AGPL-3.0 |
| [helm-charts](https://github.com/lockdep/helm-charts) | The `stackradar` chart: both apps, a bundled Postgres and the scanner | Apache-2.0 |
| [stackradar-scanner](https://github.com/lockdep/stackradar-scanner) | The read-only in-cluster agent that writes one SBOM per image with [Syft](https://github.com/anchore/syft) | Apache-2.0 |
| [stackradar-cli](https://github.com/lockdep/stackradar-cli) | `stackradar scan`: the same answer from a laptop, no account, no agent | Apache-2.0 |
| [homebrew-tap](https://github.com/lockdep/homebrew-tap) | `brew install lockdep/tap/stackradar` | — |

## Install

**Self-hosted.** Kubernetes 1.25+ and Helm 3.8+. No values file, no database to bring, and the scanner is included:

```bash
helm install stackradar oci://ghcr.io/lockdep/helm-charts/stackradar \
  --create-namespace -n stackradar
```

The [README](https://github.com/lockdep/stackradar#quick-start) has the Docker Compose try-out and the [self-hosting docs](https://github.com/lockdep/stackradar/tree/main/docs/self-hosting) cover your own Postgres, OIDC, upgrades and backups.

**StackRadar Cloud.** Install only the scanner, with the cluster key from the dashboard:

```bash
helm install stackradar-scanner oci://ghcr.io/lockdep/charts/stackradar-scanner \
  --version <version> \
  --namespace stackradar --create-namespace
```

Always pin an explicit `--version`; published versions are immutable. The latest is on the [scanner releases page](https://github.com/lockdep/stackradar-scanner/releases/latest).

**From a laptop.** `brew install lockdep/tap/stackradar`, then `stackradar scan` against any cluster you have `kubectl` access to.

## How it works

The scanner watches pods in real time, pulls each image once per digest and reports a CycloneDX SBOM plus the workload inventory over an API key scoped to its cluster. The server mirrors [OSV.dev](https://osv.dev), [EPSS](https://www.first.org/epss/) and the [CISA KEV catalogue](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) locally and matches every stored SBOM again whenever the mirror changes, so every finding can be checked against its public advisory independently of us. The only thing that leaves the cluster is the SBOM and the metadata to attribute it: never source code, image layers, secrets or credentials. Details on [stackradar.io/security](https://stackradar.io/security).

## Audit what you install

Every release is signed with Sigstore Cosign, built in GitHub Actions with SLSA provenance and SBOM attestations, and the charts pin images by `sha256` digest. Report a vulnerability privately through [GitHub's private reporting](https://github.com/lockdep/stackradar/security/advisories/new) or to security@stackradar.io.

---

[stackradar.io](https://stackradar.io) · [Docs](https://stackradar.io/docs) · [Discussions](https://github.com/lockdep/stackradar/discussions) · [Security](https://stackradar.io/security)
