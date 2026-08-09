# Common GitHub Actions

Reusable workflows for My Agent World repositories.

The repository standard is:

- CI builds and validates artifacts.
- Container workflows publish immutable `${{ github.sha }}` tags.
- GitOps repositories receive manifest changes; workflows do not mutate a Kubernetes cluster.
- Argo CD observes Git and performs reconciliation.

Callers must pin this repository to a release tag or commit SHA, never a moving branch. Releases use patch versions by default.

## Workflows

- `docker-build-push.yml`: BuildKit container build and registry push.
- `npm-publish.yml`: Verify and publish an npm package on a release.
- `kustomize-validate.yml`: Render Kustomize without cluster access.
- `node-ci.yml`: Install locked Node dependencies and run one repository verification command.
- `gitops-policy.yml`: Reject direct Kubernetes mutation commands in workflows.
- `validate-workflows.yml`: Lint workflow syntax with actionlint.

All reusable workflows require explicit permissions and expose only the inputs needed by the caller.

Callers use the same thin-wrapper shape:

```yaml
jobs:
  ci:
    uses: My-Agent-World/common-github-actions/.github/workflows/node-ci.yml@v0.1.0
    with:
      node-version: '22.x'
      verify-command: npm run verify
```

Deployment workflows may build and publish immutable container images, but must update Git manifests for Argo CD. They must not call `kubectl apply`, `helm upgrade`, `argocd app sync`, or otherwise mutate a cluster.
