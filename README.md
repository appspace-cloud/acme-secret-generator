# ACME Secret Generator Helm Chart

ACME Secret Generator is a simple Helm chart that creates a Kubernetes namespace. This chart is designed to be minimal and focused solely on namespace creation.

## Overview

This Helm chart can create a single Kubernetes namespace with the same name as the chart (`acme-secret-generator`). By default, namespace creation is disabled to allow external tools (like Terraform) to manage the namespace.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+

## Installation

### From Public Helm Repository (Recommended)

Add the repository:
```bash
helm repo add acme-secret-generator https://appspace-cloud.github.io/acme-secret-generator
helm repo update
```

Install the chart (with namespace creation by Helm):
```bash
helm install acme-secret-generator acme-secret-generator/acme-secret-generator --namespace acme-secret-generator --create-namespace --set createNamespace=true
```

Install the chart (namespace created externally, e.g., by Terraform):
```bash
helm install acme-secret-generator acme-secret-generator/acme-secret-generator --namespace acme-secret-generator
```

### From Local Chart

```bash
helm install acme-secret-generator . --namespace acme-secret-generator --create-namespace
```

### Installation with Custom Values

```bash
helm install acme-secret-generator acme-secret-generator/acme-secret-generator -f custom-values.yaml --namespace acme-secret-generator --create-namespace
```

## Configuration

The following table lists the configurable parameters and their default values:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `name` | Name of the release | `acme-secret-generator` |
| `namespace` | Kubernetes namespace name | `acme-secret-generator` |
| `createNamespace` | Whether to create the namespace (set to `false` if namespace is managed externally) | `false` |

### Example: Custom Configuration

```yaml
name: "acme-secret-generator"
namespace: "my-custom-namespace"
createNamespace: true  # Set to false if namespace is created by Terraform or other tools
```

### Using with Terraform

When using Terraform to manage the namespace, set `createNamespace: false` (default):

```hcl
helm_release "acme-secret-generator" {
  name       = "acme-secret-generator"
  repository = "https://appspace-cloud.github.io/acme-secret-generator"
  chart      = "acme-secret-generator"
  version    = "0.1.1"
  namespace  = "acme-secret-generator"
  
  # Namespace is created by Terraform, not by Helm
  create_namespace = false
  values = [
    yamlencode({
      createNamespace = false
    })
  ]
}
```

## Resources Created

This chart can create the following Kubernetes resources (depending on configuration):

1. **Namespace**: Creates a namespace with the specified name (default: `acme-secret-generator`) - **only if `createNamespace: true`**

## Uninstallation

To uninstall the chart:

```bash
helm uninstall acme-secret-generator --namespace acme-secret-generator
```

**Note**: 
- This will remove the namespace created by this Helm release
- If the namespace contains other resources, they will also be removed

## Public Repository

This chart is available from the public Helm repository:

```bash
helm repo add acme-secret-generator https://appspace-cloud.github.io/acme-secret-generator
helm repo update
helm search repo acme-secret-generator
```

Repository URL: https://appspace-cloud.github.io/acme-secret-generator

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

Copyright (c) AppSpace. All rights reserved.

