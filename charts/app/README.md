# App Helm Chart

A Helm chart for deploying a Kubernetes application with comprehensive configuration options.

## Features

This chart provides a flexible deployment template with support for:

- **Image Configuration**: Customize the container image, tag, and pull policy
- **Image Pull Secrets**: Configure secrets for private container registries
- **Pod Annotations & Labels**: Add custom annotations and labels to pods
- **Environment Variables**: Set environment variables from values or Kubernetes secrets
- **Health Checks**: Configure liveness and readiness probes (HTTP, TCP, or exec)
- **Service Account**: Optional service account creation with custom annotations
- **Resource Management**: Configure resource requests and limits
- **Scheduling**: Support for node selectors, tolerations, and affinity rules

## Installation

```bash
helm install my-release charts/app
```

## Configuration

The following table lists the main configurable parameters:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Container image repository | `nginx` |
| `image.tag` | Container image tag | `""` (uses appVersion) |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `imagePullSecrets` | Image pull secrets | `[]` |
| `podAnnotations` | Annotations for pods | `{}` |
| `podLabels` | Labels for pods | `{}` |
| `env` | Environment variables | `[]` |
| `livenessProbe.enabled` | Enable liveness probe | `true` |
| `readinessProbe.enabled` | Enable readiness probe | `true` |
| `service.type` | Service type | `ClusterIP` |
| `service.port` | Service port | `80` |
| `serviceAccount.create` | Create service account | `true` |
| `serviceAccount.annotations` | Service account annotations | `{}` |

## Examples

### Custom Image and Environment Variables

```yaml
image:
  repository: myapp
  tag: "1.2.3"
  pullPolicy: Always

env:
  - name: DATABASE_URL
    value: "postgresql://db:5432/mydb"
  - name: API_KEY
    valueFrom:
      secretKeyRef:
        name: api-secrets
        key: api-key
```

### Custom Health Checks

```yaml
livenessProbe:
  enabled: true
  httpGet:
    path: /healthz
    port: http
  initialDelaySeconds: 60
  periodSeconds: 15

readinessProbe:
  enabled: true
  httpGet:
    path: /ready
    port: http
  initialDelaySeconds: 10
  periodSeconds: 5
```

### Image Pull Secrets

```yaml
imagePullSecrets:
  - name: my-registry-secret
```

### Custom Annotations and Labels

```yaml
podAnnotations:
  prometheus.io/scrape: "true"
  prometheus.io/port: "8080"

podLabels:
  environment: production
  team: backend
```

## Uninstalling

```bash
helm uninstall my-release
```
