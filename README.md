# Compose Deployment Technology Specification

This repository contains the supplemental requirements for application and workload target suppliers to support the Compose workload deployment technology. This documentation supplements the requirements documented in Margo's [core specification](https://github.com/margo/specification) and does not stand on its own.

**Deployment Technology Key:** `margo.org/deployment/compose`

**Status:** Candidate

**Specification Version:** `1.0.0-rc.3`

> **Note:** This specification follows the same [Versioning Management Strategy](https://docs.margo.org/specification/specification-version-management) outlined in Margo's core specification. This specification is versioned independently of Margo's core specification.

## Supplemental Requirements

The following pages define the additional requirements for supporting the Compose deployment technology:

- [Compose Profile Characteristics](./specification//margo-management-interface/compose-profile-characteristics.md)
- [Application Description Supplemental Material](./specification/applications/application-description.md)
- [Application Registry Supplemental Material](./specification/applications/application-registry.md)
- [Workload Target Requirements Supplemental Material](./specification/margo-workload-targets/workload-target-requirements.md)
- [Collecting Workload Observability Data Supplemental Material](./specification/observability/collecting-workload-observability-data.md)

## Local Registries

This section describes options for configuring local registries to avoid reliance on public Internet-accessible registries for situations where the Application Package and workload artifacts are not accessible via the internet.

### Option - Container Image Registry as Pull-through Cache on Docker Level

To configure a pull-through cache in Docker for the Container Image Registry, a Docker Registry can be set up that acts as a caching proxy for a remote Docker Registry. Such a Docker Registry can be defined using the following `config.yml`:

```yaml
version: 0.1
log:
  fields:
    service: registry
storage:
  filesystem:
    rootdirectory: /var/lib/registry
http:
  addr: :5000
  headers:
    X-Content-Type-Options: [nosniff]
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
proxy:
  remoteurl: https://registry-1.docker.io
```

This registry can be started using the following command:
`docker run -d -p 5000:5000 --restart unless-stopped --name registry -v $(pwd)/config.yml:/etc/docker/registry/config.yml registry:2`

Then, the Docker daemon needs to be configured to use the private registry as a mirror in the `/etc/docker/daemon.json` file:

```json
{
  "registry-mirrors": ["http://10.100.2.102:5000"],
  "insecure-registries": ["10.100.2.102:5000"]
}
```

## Licenses

Margo contributors contributing to this draft specification have made those contributions subject to the Open Web Foundation Contributor License Agreement Contribution Copyright Grant, available at [Open Web Foundation - CLA Copyright Grant 0.9](https://www.openwebfoundation.org/the-agreements/the-owf-0-9-agreements-necessary-claims/cla-copyright-grant-0-9).

Copyright ©️ 2025 Margo.
