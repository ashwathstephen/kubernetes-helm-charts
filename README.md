# Kubernetes Helm Charts

[![Helm](https://img.shields.io/badge/Helm-3.0+-0F1689?logo=helm)](https://helm.sh/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.25+-326CE5?logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Production-ready Helm charts for deploying applications on Kubernetes. Built with best practices for security, reliability, and scalability.

## Available Charts

| Chart | Description | Use Case |
|-------|-------------|----------|
| [microservice](./charts/microservice) | Deployment with HPA, PDB, Ingress | Web services, APIs |
| [cronjob](./charts/cronjob) | Scheduled job execution | Batch jobs, cleanup tasks |
| [statefulset](./charts/statefulset) | Stateful applications with PVC | Databases, message queues |

## Features

- Security: Pod security contexts, read-only filesystems, non-root users
- High Availability: Pod anti-affinity, PDB, multi-replica deployments
- Autoscaling: HPA with CPU/memory metrics and custom behavior
- Observability: ServiceMonitor for Prometheus integration
- Flexibility: Comprehensive values for customization

## Quick Start

### Install a Chart

```bash
# Add this repository (if published)
helm repo add devops https://ashwathstephen.github.io/kubernetes-helm-charts

# Install microservice chart
helm install my-app ./charts/microservice \
  --set image.repository=myorg/myapp \
  --set image.tag=v1.0.0

# Install with custom values file
helm install my-app ./charts/microservice -f my-values.yaml
```

### Local Development

```bash
# Lint charts
helm lint charts/microservice
helm lint charts/cronjob
helm lint charts/statefulset

# Template rendering (dry-run)
helm template my-app ./charts/microservice

# Install locally with Kind cluster
kind create cluster
helm install my-app ./charts/microservice
```

## Chart Documentation

### Microservice Chart

Deploy stateless microservices with:
- Deployment with rolling updates
- HorizontalPodAutoscaler (HPA)
- PodDisruptionBudget (PDB)
- Service and Ingress
- ConfigMap and Secret management
- ServiceMonitor for Prometheus

Example values:

```yaml
replicaCount: 3

image:
  repository: myorg/myapp
  tag: "v1.0.0"

resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 100m
    memory: 128Mi

autoscaling:
  enabled: true
  minReplicas: 2
  maxReplicas: 10

ingress:
  enabled: true
  className: nginx
  hosts:
    - host: myapp.example.com
      paths:
        - path: /
          pathType: Prefix
```

### CronJob Chart

Deploy scheduled jobs with:
- Configurable schedule (cron format)
- Concurrency policy
- Job history limits
- Retry configuration

Example values:

```yaml
schedule: "0 2 * * *"  # Daily at 2 AM

image:
  repository: myorg/backup-job
  tag: "v1.0.0"

command:
  - /bin/sh
  - -c
  - "backup.sh"

resources:
  limits:
    cpu: 500m
    memory: 512Mi
```

### StatefulSet Chart

Deploy stateful applications with:
- Ordered pod management
- Persistent volume claims
- Headless service for DNS
- Rolling update strategy

Example values:

```yaml
replicaCount: 3

image:
  repository: postgres
  tag: "15"

persistence:
  enabled: true
  size: 20Gi
  storageClass: gp3

resources:
  limits:
    cpu: 1000m
    memory: 2Gi
```

## Testing Charts

```bash
# Lint all charts
for chart in charts/*; do
  helm lint "$chart"
done

# Template and validate
helm template test ./charts/microservice | kubectl apply --dry-run=client -f -

# Install in test namespace
helm install test ./charts/microservice -n test --create-namespace
```

## Author

Ashwath Abraham Stephen
Senior DevOps Engineer | [LinkedIn](https://linkedin.com/in/ashwathstephen) | [GitHub](https://github.com/ashwathstephen)

## License

MIT License - see [LICENSE](LICENSE) for details.

