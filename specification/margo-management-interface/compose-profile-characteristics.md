# Compose Profile Characteristics

The following [Profile](https://docs.margo.org/specification/margo-management-interface/) characteristics are applicable for workload targets supporting the Compose deployment technology. Workload targets supporting the Compose deployment technology MUST report the applicable characteristics as defined below in the profile.

## Compose Deployment Technology Support

The following required characteristics indicates a workload target is capable of deploying Compose workloads.

Key: `margo.org/deployment/compose`

### Compose Deployment Technology Properties Attributes

> **Note:** The properties for the Compose deployment technology will be defined once the deployment technology specifications are separated from the core specification.

### Compose Deployment Technology Example

```json
{
    "key": "margo.org/deployment/compose",
    "properties": {}
}
```

## Additional Required Characteristics

The following characteristics MUST be reported for workload targets supporting the Compose deployment technology.

### CPUs

Key: `margo.org/resource/cpu`

#### CPUs Properties Attributes

| Field      | Type            | Required?       | Description     |
|------------|-----------------|-----------------|-----------------|
| cores | number | Y | Defines how many cores the CPU has available. Specified as decimal units of CPU cores (e.g., 0.5 is half a core). This is defined by the workload target owner. |
| architecture | enum | N | Define the CPU's architecture. Must be `amd64`, `arm64` or `arm` |

#### CPUs Example

```json
{
    "key": "margo.org/resource/cpu",
    "properties": {
        "cpus": [{
            "cores": 12,
            "architecture": "amd64"
        },{
            "cores": 12,
            "architecture": "amd64"   
        }]
    }
}
```

### Memory

Key: `margo.org/resource/memory`

#### Memory Properties Attributes

| Field      | Type            | Required?       | Description     |
|------------|-----------------|-----------------|-----------------|
| available | number | Y | The amount of memory, in Gibibytes, available for workloads to utilize on the workload target. |

#### Memory Example

```json
{
    "key": "margo.org/resource/memory",
    "properties": {
        "available": 64
    }
}
```

### Storage

Key: `margo.org/resource/storage`

#### Storage Properties Attributes

| Field      | Type            | Required?       | Description     |
|------------|-----------------|-----------------|-----------------|
| available | number | Y | The amount of storage, in Gibibytes, available for workloads to utilize on the workload target. |

#### Storage Example

```json
{
    "key": "margo.org/resource/storage",
    "properties": {
        "available": 1862
    }
}
```

### Peripherals

Indicates the peripherals available on the workload target.

Key: `margo.org/peripherals`

#### Peripherals Properties Attributes

| Field      | Type            | Required?       | Description     |
|------------|-----------------|-----------------|-----------------|
| type | enum | Y | The type of peripheral available on the workload target. Must be `gpu`, `display`, `camera`, `microphone` or `speaker` |
| manufacturer | string | N | The name of the manufacturer. |
| model | string | N | The model of the peripheral. |

#### Peripherals Example

```json
{
    "key": "margo.org/peripherals",
    "properties": {
        "peripherals": [{
            "type": "gpu",
            "manufacturer": "NVIDIA",
        },{
            "type": "microphone"
        }]
    }
}
```

### Interfaces

Indicates the interfaces available on the workload target.

Key: `margo.org/interfaces`

#### Interfaces Properties Attributes

| Field      | Type            | Required?       | Description     |
|------------|-----------------|-----------------|-----------------|
| type | enum | Y | The type of interfaces available on the workload target. Must be `ethernet`, `wifi`, `cellular`, `bluetooth`, `usb`, `canbus`, or `rs232` |

#### Interfaces Example

```json
{
    "key": "margo.org/interfaces",
    "properties": {
        "interfaces": [{
            "type": "ethernet",
        },{
            "type": "wifi"
        }]
    }
}
```

## Example Profile Payload

```json
{
    "characteristics": [{
        "key": "margo.org/deployment/compose",
        "properties": {}
    },{
        "key": "margo.org/resource/cpu",
        "properties": {
            "cpus": [{
                "cores": 24,
                "architecture": "amd64"
            }]
        }
    },{
        "key": "margo.org/resource/memory",
        "properties": {
            "available": 59
        }
    },{
        "key": "margo.org/resource/storage",
        "properties": {
            "available": 1862
        }
    },{
        "key": "margo.org/peripherals",
        "properties": {
            "peripherals": [{
                "type": "gpu",
                "manufacturer": "NVIDIA"
            }]
        }
    },{
        "key": "margo.org/interfaces",
        "properties": {
            "interfaces": [{
                "type": "ethernet"
            },{
                "type": "wifi"
            }]
        }
    }]
}
```
