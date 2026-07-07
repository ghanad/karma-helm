# karma-helm

Helm chart for deploying [Karma](https://github.com/prymitive/karma) in Kubernetes.

## Install

```bash
helm install karma .
```

By default the chart creates a Deployment, Service, ConfigMap, and ServiceAccount. Ingress is disabled by default.

## Configure Alertmanager

Karma needs at least one Alertmanager server to be useful:

```yaml
config: |
  alertmanager:
    servers:
      - name: production
        uri: http://alertmanager.monitoring.svc:9093
```

Install with custom values:

```bash
helm install karma . -f values-prod.yaml
```

## Enable Ingress

```yaml
ingress:
  enabled: true
  ingressClassName: nginx
  hosts:
    - host: karma.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: karma-tls
      hosts:
        - karma.example.com
```

## Important Values

| Value | Default | Description |
| --- | --- | --- |
| `replicaCount` | `1` | Number of Karma pods. |
| `image.repository` | `ghcr.io/prymitive/karma` | Container image repository. |
| `image.tag` | `""` | Image tag. Empty means chart `appVersion`. |
| `service.port` | `80` | Kubernetes Service port. |
| `service.targetPort` | `http` | Named container port used by the Service. |
| `ingress.enabled` | `false` | Create an Ingress resource. |
| `automountServiceAccountToken` | `false` | Avoid mounting Kubernetes API credentials into the pod. |
