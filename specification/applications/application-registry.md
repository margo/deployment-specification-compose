# Application Registry Supplemental Material

The following content supplements the [Application Registry](https://docs.margo.org/specification/applications/application-registry) documentation in the core specification with requirements specific to how the Composed OCI blob is created to facilitate workload deployment.

## Compose Component Registry

Compose components MUST be stored in an OCI-compliant Component Registry and referenced via `repository` (an `oci://` URI) and `revision` (an OCI tag matching SemVer 2.0) in the ApplicationDescription and ApplicationDeployment manifests.

The OCI image manifest for a Compose component MUST use `artifactType` = `application/vnd.org.margo.component.compose+json`. The single layer blob MUST use `mediaType` = `application/vnd.org.margo.component.compose.tar+gzip`.

### Compose-Specific Media Types

|Media Type|Description|
|----------|----------|
|``application/vnd.org.margo.component.compose+json``| MUST be used as the **artifactType** in the OCI image manifest for a Margo Compose Archive |
|``application/vnd.org.margo.component.compose.tar+gzip``| MUST be used as the layer blob **mediaType** for a Margo Compose Archive |

### Compose Component Annotation Keys

The following annotations MUST be set on the layer descriptor when pushing a Compose Archive to an OCI registry:

| Annotation Key | Required? | Description |
|----------------|-----------|-------------|
| `org.margo.component.type` | REQUIRED | MUST be `compose`. Matches the `DeploymentProfile.type` enum value. |
| `org.margo.component.version` | REQUIRED | SemVer 2.0 version without leading `v`. MUST match the OCI tag and `ComponentProperties.revision`. |
| `org.opencontainers.image.title` | RECOMMENDED | Human-readable component name. |
| `org.opencontainers.image.authors` | RECOMMENDED | Author or organization (e.g., `ACME Corp (maintainer@example.com)`). |
| `org.opencontainers.image.description` | RECOMMENDED | Short description of the component. |
| `org.opencontainers.image.version` | RECOMMENDED | MUST equal `org.margo.component.version` if set. |

These annotations enable registry UIs and tooling to discover and filter Margo artifacts without pulling the blob content.

### Compose Archive Structure

A Compose Archive is a gzip-compressed tar archive (`.tar.gz` or `.tgz`) that packages a Compose application for deployment on edge devices. The archive MUST conform to the following structural requirements.

#### Directory Layout

The archive MUST contain exactly one top-level directory.

The directory name SHOULD match the component `name` as specified in the ApplicationDescription for human readability, but implementations MUST NOT depend on the directory name for discovery.

Discovery algorithm: enter the single top-level directory; locate the file named `compose.yaml`.

The top-level directory MAY contain any number of subdirectories (e.g., `configs/`, `certs/`, `scripts/`). All referenced files MUST resolve within the top-level directory.

The top-level directory MUST contain a file named `compose.yaml`. The Compose file MUST conform to the Compose Specification as currently published.

> **Note:** The Compose file MUST be named `compose.yaml`. The alternative names `compose.yml`, `docker-compose.yaml`, and `docker-compose.yml` are NOT valid within a Margo Compose Archive.

Files referenced by `compose.yaml` via `env_file` entries and `configs` (file source) MUST be included within the archive and MUST be referenced using relative paths that resolve within the top-level directory.

Bind-mount volume paths declared in `volumes` are runtime paths and MUST NOT be included in the archive.

Files for `secrets` (file source) MUST NOT be included in the archive. Secret provisioning is out of scope and is the responsibility of the device or WFM implementation at deployment time.

#### Security Constraints

- Symlinks MUST NOT target paths outside the top-level directory.
- Hard links MUST NOT reference paths outside the top-level directory.
- Absolute paths MUST NOT appear in the archive entries.
- File names MUST NOT contain path traversal sequences (`../`).
- Implementations SHOULD normalize file permissions during archive extraction. Implementations MUST NOT preserve setuid, setgid, or sticky bits from archive entries.
- WFM and device implementations MUST validate these constraints before extracting or deploying the archive.

#### Integrity Verification

When stored in an OCI-compliant Component Registry, the Compose Archive tarball is the content of a single layer blob. Integrity verification at the transport layer is provided by the OCI content-addressable digest as mandated by the [OCI Distribution Specification v1.1.0](https://github.com/opencontainers/distribution-spec/blob/v1.1.0/spec.md). Implementations MUST verify the OCI digest after pulling the blob and before extracting the archive.

### Publishing Workflow

To publish a Compose Archive to an OCI-compliant Component Registry, implementations MAY use any OCI-compliant push tool. The example below uses `oras push` ([ORAS — OCI Registry as Storage](https://oras.land/)), which is a RECOMMENDED tool for Margo Compose Archives.

> **Warning**: `docker compose publish` (Docker Compose 2.34.0+) MUST NOT be used to publish Margo Compose components. It produces a structurally incompatible OCI artifact: `artifactType: application/vnd.docker.compose.project`, multiple layers (one per file), and SHA256-hashed file paths. This format cannot be consumed by a Margo-compliant WFM or device implementation.

Example:

```bash
oras push registry.example.com/org/myapp:1.0.0 \
  --artifact-type application/vnd.org.margo.component.compose+json \
  myapp-1.0.0-compose.tar.gz:application/vnd.org.margo.component.compose.tar+gzip \
  --annotation "org.margo.component.type=compose" \
  --annotation "org.margo.component.version=1.0.0" \
  --annotation "org.opencontainers.image.title=myapp" \
  --annotation "org.opencontainers.image.version=1.0.0" \
  --annotation "org.opencontainers.image.authors=ACME Corp (maintainer@example.com)" \
  --annotation "org.opencontainers.image.description=My application workload"
```

Reference the artifact in the ApplicationDescription:

```yaml
components:
  - name: myapp
    properties:
      repository: oci://registry.example.com/org/myapp
      revision: "1.0.0"
```
