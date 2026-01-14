# helm-rabbitmq-cluster-operator

Crossplane configuration that installs the RabbitMQ Cluster Operator Helm chart with a minimal, stable interface.

## Overview

This configuration provides a Crossplane XRD for deploying the RabbitMQ Cluster Operator using the Helm provider. The RabbitMQ Cluster Operator automates provisioning, management, and operations of RabbitMQ clusters running on Kubernetes.

## Usage

### Minimal Example

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: RabbitmqClusterOperator
metadata:
  name: rabbitmq-cluster-operator
  namespace: my-namespace
spec:
  clusterName: my-cluster
```

### Standard Example

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: RabbitmqClusterOperator
metadata:
  name: rabbitmq-cluster-operator
  namespace: my-namespace
spec:
  clusterName: my-cluster
  namespace: rabbitmq-system
  labels:
    team: platform
    environment: production
  providerConfigRef:
    name: my-cluster
    kind: ProviderConfig
  values:
    clusterOperator:
      replicaCount: 1
    msgTopologyOperator:
      replicaCount: 1
```

## Configuration Options

| Field | Description | Default |
|-------|-------------|---------|
| `clusterName` | Target cluster name (used for provider config) | Required |
| `namespace` | Kubernetes namespace for the release | `rabbitmq-system` |
| `name` | Helm release name | XR metadata.name |
| `labels` | Labels applied to managed resources | See below |
| `providerConfigRef.name` | Helm ProviderConfig name | `clusterName` |
| `providerConfigRef.kind` | ProviderConfig kind | `ProviderConfig` |
| `values` | Helm values (merged with defaults) | `{}` |
| `overrideAllValues` | Helm values (replaces all defaults) | `{}` |
| `managementPolicies` | Crossplane management policies | `["*"]` |

### Default Labels

```yaml
hops.ops.com.ai/managed: "true"
hops.ops.com.ai/rabbitmqclusteroperator: "<name>"
```

## Helm Chart

- **Chart**: rabbitmq-cluster-operator
- **Repository**: oci://registry-1.docker.io/bitnamicharts
- **Version**: 4.4.34

## Dependencies

- Provider: crossplane-contrib/provider-helm >= v1.0.6
- Function: crossplane-contrib/function-auto-ready >= v0.6.0
