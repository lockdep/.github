# StackRadar

**The vulnerabilities on the images running in your Kubernetes cluster, ranked by what to fix first, with the upgrade that fixes them.**

A read-only agent watches the pods in your cluster and writes one SBOM per image with [Syft](https://github.com/anchore/syft). The server mirrors [OSV.dev](https://osv.dev), [EPSS](https://www.first.org/epss/) and the [CISA KEV catalogue](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) and matches every stored SBOM again whenever the mirror changes. A new advisory shows up against the pods that run it without pulling or scanning anything twice. Every finding is tied to its Helm release and Argo CD application, so the fix is a chart version, not a CVE id.

StackRadar is open source. Self-host it under AGPL-3.0, or use the hosted version at [app.stackradar.io](https://app.stackradar.io).

## Repositories

| Repository | What it is | Licence |
|:--|:--|:--|
| [**stackradar**](https://github.com/lockdep/stackradar) | The server, the dashboard and the self-hosting manual. Start here. | AGPL-3.0 |
| [helm-charts](https://github.com/lockdep/helm-charts) | The `stackradar` chart: both apps, a bundled Postgres and the scanner | Apache-2.0 |
| [stackradar-scanner](https://github.com/lockdep/stackradar-scanner) | The in-cluster agent, the only SBOM ingestion path | Apache-2.0 |
| [stackradar-cli](https://github.com/lockdep/stackradar-cli) | `stackradar scan`: the same answer from your laptop, no account, no agent | Apache-2.0 |
| [homebrew-tap](https://github.com/lockdep/homebrew-tap) | Homebrew formulae for the CLI | Apache-2.0 |

## Self-host in one command

Kubernetes 1.25+ and Helm 3.8+. No values file, no database to bring, and the scanner is included:

```sh
helm install stackradar oci://ghcr.io/lockdep/helm-charts/stackradar \
  --create-namespace -n stackradar
kubectl -n stackradar port-forward svc/stackradar-client 3000:3000
```

The [stackradar README](https://github.com/lockdep/stackradar#quick-start) has the rest: the Docker Compose try-out, the laptop CLI, and the warning about the fixture credentials the default install ships with. If you would rather click first, there is a [read-only live demo](https://demo.stackradar.io/demo/enter).

## Audit what you install

The agent runs with cluster-wide read access, so its source is published for you to read, and what leaves the cluster is an [allowlist](https://github.com/lockdep/stackradar-scanner#what-leaves-your-cluster). Every release is signed with Sigstore cosign, built in GitHub Actions with SLSA provenance and SBOM attestations, and the charts pin images by digest. A self-hosted install sends no telemetry; the advisory mirror syncs are its only outbound traffic.

Report a vulnerability privately through [GitHub's private reporting](https://github.com/lockdep/stackradar/security/advisories/new) or to security@stackradar.io. [SECURITY.md](https://github.com/lockdep/stackradar/blob/main/SECURITY.md) has the scope and the response time.

---

[stackradar.io](https://stackradar.io) · [Docs](https://stackradar.io/docs) · [Self-hosting manual](https://github.com/lockdep/stackradar/tree/main/docs/self-hosting) · [Discussions](https://github.com/lockdep/stackradar/discussions) · [Security](https://stackradar.io/security)
