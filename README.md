# ovirt-workflows

Shared GitHub Actions reusable workflows for oVirt projects.

## Usage

Call a workflow from any oVirt repository using `uses:` and pass all required
secrets with `secrets: inherit`:

```yaml
jobs:
  publish-rpms:
    needs: build
    if: ${{ github.event_name == 'push' || github.event_name == 'workflow_dispatch' }}
    uses: ovirt/ovirt-workflows/.github/workflows/publish-rpms.yml@main
    secrets: inherit
```

## Available workflows

### `publish-rpms.yml` — Publish RPMs to resources.ovirt.org

Downloads built RPM artifacts for all supported distributions and publishes
them to `resources.ovirt.org` under `ovirt-master-snapshot/`.

**Distributions covered:** AlmaLinux 9 (`el9`), AlmaLinux 10 (`el10`),
CentOS Stream 9 (`el9s`), CentOS Stream 10 (`el10s`).

**Required secrets** (provided via `secrets: inherit`):

| Secret | Description |
|--------|-------------|
| `SSH_USERNAME_FOR_RESOURCES_OVIRT_ORG` | SSH username for resources.ovirt.org |
| `SSH_KEY_FOR_RESOURCES_OVIRT_ORG` | SSH private key for resources.ovirt.org |
| `KNOWN_HOSTS_FOR_RESOURCES_OVIRT_ORG` | Known hosts entry for resources.ovirt.org |

**Artifact naming convention:** the calling project's build job must upload
artifacts named `rpm-<shortcut>` (e.g. `rpm-el9`) using
`actions/upload-artifact`.
