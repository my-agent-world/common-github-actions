# Common GitHub Actions

Reusable workflows for My Agent World repositories.

The repository standard is:

- CI builds and validates artifacts.
- Container workflows publish immutable `${{ github.sha }}` tags.
- GitOps repositories receive manifest changes; workflows do not mutate a Kubernetes cluster.
- Argo CD observes Git and performs reconciliation.

Callers should pin this repository to a release tag or commit SHA. The first release will be a patch release after the workflows have been exercised by representative repositories.

## Workflows

- `docker-build-push.yml`: BuildKit container build and registry push.
- `npm-publish.yml`: Verify and publish an npm package on a release.
- `kustomize-validate.yml`: Render Kustomize without cluster access.
- `gitops-policy.yml`: Reject direct Kubernetes mutation commands in workflows.

All reusable workflows require explicit permissions and expose only the inputs needed by the caller.
