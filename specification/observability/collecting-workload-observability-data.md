# Collecting Workload Observability Data Supplemental Material

The following content supplements the [Collecting Workload Observability Data](https://docs.margo.org/specification/observability/collecting-workload-observability-data) documentation in the core specification with requirements clarifications specific for the Compose deployment technology.

## Compose Platform Observability Requirements

For workload targets with runtimes supporting Compose deployment technologies, such as Docker or Podman, the following is a minimum list of observability data that MUST be provided. The device owner MAY choose to provide additional observability data if they wish.

- Container observability data MUST be collected.
  - It is recommended the Device Owner use the [Docker Stats Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/dockerstatsreceiver/README.md) or [Podman Stats Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/podmanreceiver/README.md) with the default configuration to collect this information but using either of these receivers is not required.
  - If the Device Owner chooses not to use either receiver they MUST provide the same output as the receiver's default configuration.

  > **Note:** Please see the [information below](#workload-observability-default-telemetry) for the default metrics emitted by the Docker Stats and Podman Stats Receivers.

## Workload Observability Default Telemetry

The following telemetry data is collected by using the default configurations for the receivers indicated above. You can find more information about each piece of telemetry from the receiver's documentation.

### Metrics

The following table shows the metrics emitted by the indicated receivers when using the default configuration.

| Metric Group   |  Metric  | Target | [Docker Stats Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/dockerstatsreceiver) | [Podman Stats Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/podmanreceiver) | [Host Metrics Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/hostmetricsreceiver) |
|----------|---------|-------|----------------|--------------------------|-------------------------|
| CPU | Load Average (15m, 5m, 1m) | System |  |  | X |
| CPU | Time | Container, System |  |  | X |
| CPU | Usage Kernel Mode | Container | X |  |  |
| CPU | Usage Per CPU | Container |  | X |  |
| CPU | Usage System | Container |  | X |  |
| CPU | Usage Total | Container | X | X |  |
| CPU | Usage Use Mode | Container | X |  |  |
| CPU | Utilization | Container | X | X |  |
| Disk | IO | Container, System |  | X | X |
| Disk | IO Read | Container |  | X |  |
| Disk | IO Write | Container |  | X |  |
| Disk | IO Time | System |  | X |  |
| Disk | IO Time (Weighted) | System  |  | X |  |
| Disk | Operations | System |  | X |  |
| Disk | Operations Pending | System  |  | X |  |
| Disk | Operation Time | System |  | X |  |
| Disk | Total Read/Writes  |  System |  | X |  |
| File System | Inodes | System |  | X |  |
| File System | Usage | Container, System |  |  | X |
| Memory | File | Container | X |  |  |
| Memory | Limit | Container | X | X |  |
| Memory | Percent | Container | X | X |  |
| Memory | Total Cache | Container | X |  |  |
| Memory | Usage | Container, System | X | X | X |
| Network | Connections | System |  |  | X |
| Network | Errors | System |  |  | X |
| Network | IO | System |  |  | X |
| Network | IO Bytes Sent | Container | X | X |  |
| Network | IO Bytes Received | Container | X | X |  |
| Network | IO Packets | System |  |  | X |
| Network | IO Packets Dropped | System |  |  | X |
| Network | IO Packets Dropped (Incoming) | Container | X |  |  |
| Network | IO Packets Dropped (Outgoing) | Container | X |  |  |
| Paging | Faults | System |  |  | X |
| Paging | Operations | System |  |  | X |
| Paging | Usage | System |  |  | X |
| Process | CPU Time | System |  |  | X |
| Process | Disk IO | System |  |  | X |
| Process | Memory Usage | System |  |  X |  |
| Process | Memory Virtual | System |  | X |  |
| Processes | Count | System |  | X |
| Processes | Created | System |  |   | X |
