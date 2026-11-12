# Application Description Supplemental Material

The following content supplements the [Application Description](https://docs.margo.org/specification/applications/application-description) documentation in the core specification with requirements clarifications specific for Compose workloads.

## Application Description Property Handling

The following defines how certain properties in the Application Description file are handled specifically for Compose workloads. Workload target implementation MUST apply these rules when installing Compose workloads.

- The `component.properties.wait` if `wait` is set to `true` for a Compose component, the device MUST wait until all containers in the Compose project reach **running** state before reporting the deployment as successful. This is equivalent to `docker compose up` or `podman-compose up` completing synchronously without `--detach`. If any container exits with a non-zero exit code during startup, the deployment MUST be reported as failed immediately. If health checks are defined in `compose.yaml`, implementations SHOULD additionally wait for all containers to reach **healthy** state before reporting success.
- The `parameter.targets[].pointer` MUST be the environment variable name the Compose files expect to use.

## Example Application Description

```yaml
apiVersion: v1
id: com-northstartida-digitron-orchestrator
metadata:
  name: Digitron orchestrator
  description: The Digitron orchestrator application
  version: 1.2.1 
  catalog:
    application:
      icon: ./resources/ndo-logo.png
      tagline: Northstar Industrial Application's next-gen, AI driven, Digitron instrument orchestrator.
      descriptionFile: ./resources/description.md
      releaseNotes: ./resources/release-notes.md
      licenseFile: ./resources/license.pdf
      site: http://www.northstar-ida.com
      tags: ["optimization", "instrumentation"]
    author:
      - name: Roger Wilkershank
        email: rpwilkershank@northstar-ida.com
    organization:
      - name: Northstar Industrial Applications
        site: http://northstar-ida.com
deploymentProfiles:
  - key: margo.org/deployment/compose
    id: com-northstartida-digitron-orchestrator-compose-a
    components:
      - name: digitron-orchestrator-docker
        properties:
          repository: oci://northstarida.azurecr.io/compose/digitron-orchestrator
          revision: 1.0.9
    characteristics:
      - matchExpressions:
          - key: margo.org/resource/cpu
            property: /cpus
            operator: ContainsAny
            itemSelector:
              matchExpressions:
                - property: /cores
                  operator: GtEq
                  values: [1.5]
                - property: /architecture
                  operator: In
                  values: ["amd64"]
          - key: margo.org/resource/memory
            property: /memory
            operator: GtEq
            values: [1]
          - key: margo.org/resource/storage
            property: /storage
            operator: GtEq
            values: [10]
          - key: margo.org/peripherals
            property: /peripherals
            operator: ContainsAll
            itemSelector:
              matchExpressions:
                - property: /type
                  operator: In
                  values: ["gpu"]
                - property: /manufacturer
                  operator: In
                  values: ["NVIDIA"]
          - key: margo.org/interfaces
            property: /interfaces
            operator: ContainsAll
            itemSelector:
              matchExpressions:
                - property: /type
                  operator: In
                  values: ["ethernet", "bluetooth"]
parameters:
  idpName:
    value: "test"
    targets:
      # Maps to environment variables, e.g., IDP_NAME=test docker compose ...
      - pointer: IDP_NAME
        components: ["digitron-orchestrator-docker"]
  idpProvider:
    targets:
      - pointer: IDP_PROVIDER
        components: ["digitron-orchestrator-docker"]
  idpClientId:
    targets:
      - pointer: IDP_CLIENT_ID
        components: ["digitron-orchestrator-docker"]
  idpUrl:
    targets:
      - pointer: IDP_URL
        components: ["digitron-orchestrator-docker"]
  adminName:
    targets:
      - pointer: ADMIN_NAME
        components: ["digitron-orchestrator-docker"]
  adminPrincipalName:
    targets:
      - pointer: ADMIN_PRINCIPALNAME
        components: ["digitron-orchestrator-docker"]
  pollFrequency:
    value: 30
    targets: 
      - pointer: POLL_FREQUENCY
        components: ["digitron-orchestrator-docker"]
  siteId:
    targets:
      - pointer: SITE_ID
        components: ["digitron-orchestrator-docker"]
configuration:
  sections:
    - name: General
      settings:
        - parameter: pollFrequency
          name: Poll Frequency
          description: How often the service polls for updated data in seconds
          schema: pollRange
        - parameter: siteId
          name: Site Id
          description: Special identifier for the site (optional)
          schema: optionalText
    - name: Identity Provider
      settings:
        - parameter: idpName
          name: Name
          description: The name of the Identity Provider to use
          immutable: true
          schema: requiredText
        - parameter: idpProvider
          name: Provider
          description: Provider something something
          immutable: true
          schema: requiredText
        - parameter: idpClientId
          name: Client ID
          description: The client id
          immutable: true
          schema: requiredText
        - parameter: idpUrl
          name: Provider URL
          description: The url of the Identity Provider
          immutable: true
          schema: url
    - name: Administrator
      settings:
        - parameter: adminName
          name: Presentation Name
          description: The presentation name of the administrator
          schema: requiredText
        - parameter: adminPrincipalName
          name: Principal Name
          description: The principal name of the administrator
          schema: email
  schema:
    - name: requiredText
      dataType: string
      maxLength: 45
      allowEmpty: false
    - name: email
      dataType: string
      allowEmpty: false
      regexMatch: .*@[a-z0-9.-]*
    - name: url
      dataType: string
      allowEmpty: false
      regexMatch: ^(http(s):\/\/.)[-a-zA-Z0-9@:%._\+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)$
    - name: pollRange
      dataType: integer
      minValue: 30
      maxValue: 360
      allowEmpty: false
    - name: optionalText
      dataType: string  
      minLength: 5
      allowEmpty: true
```
