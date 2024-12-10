# WordPress Helm Chart

This repository contains a Helm chart for deploying WordPress on Kubernetes with an integrated MariaDB database. The chart provides a complete solution for running WordPress in a cloud-native environment with features like horizontal scaling, automated backups, and secure configuration management.

## Features

- WordPress deployment with configured persistent storage
- Integrated MariaDB database (using Bitnami chart)
- Horizontal Pod Autoscaling
- Ingress configuration with TLS support
- Automated plugin installation through init jobs
- Custom WordPress source code integration via Git
- Resource management and limits
- Health checks and monitoring

## Prerequisites

- Kubernetes 1.18+
- Helm 3.0+
- PV provisioner support in the underlying infrastructure
- LoadBalancer support (for Ingress controller)

## Installation

1. Add the chart repository:
```bash
helm repo add wordpress-charts https://[your-repo-url]
helm repo update
```

2. Install the chart:
```bash
helm install my-wordpress wordpress-charts/wordpress \
  --namespace my-namespace \
  --create-namespace \
  --values my-values.yaml
```

## Configuration

### Required Values

The following table lists the required parameters that must be configured in your values.yaml:

| Parameter                   | Description                              | Default |
|----------------------------|------------------------------------------|---------|
| `github.token`             | GitHub token for accessing custom code   | `""`    |
| `wordpress.adminUser`      | WordPress admin username                 | `""`    |
| `wordpress.adminPassword`  | WordPress admin password                 | `""`    |
| `wordpress.adminEmail`     | WordPress admin email                    | `""`    |
| `mariadb.auth.rootPassword`| MariaDB root password                   | `""`    |
| `mariadb.auth.password`    | MariaDB user password                   | `""`    |

### Optional Values

Other configurable values include:

- `wordpress.image.tag`: WordPress image version
- `wordpress.resources`: CPU/Memory requests and limits
- `wordpress.hpa`: Horizontal Pod Autoscaler settings
- `wordpress.ingress`: Ingress configuration
- `wordpress.pvc`: Persistent volume configuration

See `values.yaml` for full list of configurable parameters.

## Architecture

### Components

1. **WordPress Deployment**
   - Runs WordPress application
   - Uses persistent storage for WordPress files
   - Configurable resource limits and requests

2. **MariaDB StatefulSet**
   - Managed by Bitnami's MariaDB chart
   - Persistent storage for database
   - Configurable backup options

3. **Init Job**
   - Handles initial WordPress setup
   - Installs configured plugins
   - Integrates custom code from Git repository

4. **Ingress**
   - Manages external access
   - TLS termination
   - Path-based routing

### Persistence

The chart uses the following persistent volume claims:

- WordPress content: Stores themes, plugins, and uploads
- MariaDB data: Stores database files

## Security Considerations

1. **Secrets Management**
   - All sensitive data stored in Kubernetes secrets
   - Support for external secret management systems
   - Encrypted communication between components

2. **Network Security**
   - Ingress TLS configuration
   - Internal service communication only
   - Network policies available

3. **Pod Security**
   - Non-root container execution
   - ReadOnly root filesystem options
   - Resource quotas and limits

## Monitoring

The chart can be integrated with monitoring solutions:

- Prometheus metrics exposure
- WordPress status monitoring
- Database performance metrics
- Resource utilization tracking

## Maintenance

### Upgrades

To upgrade the release:
```bash
helm upgrade my-wordpress wordpress-charts/wordpress \
  --namespace my-namespace \
  --values my-values.yaml
```

### Rollbacks

To rollback to a previous release:
```bash
helm rollback my-wordpress [REVISION] --namespace my-namespace
```

### Uninstallation

To remove the deployment:
```bash
helm uninstall my-wordpress --namespace my-namespace
```

## Troubleshooting

Common issues and solutions:

1. **Pod Startup Failures**
   - Check PVC binding status
   - Verify resource quotas
   - Review init container logs

2. **Database Connection Issues**
   - Verify MariaDB credentials
   - Check network connectivity
   - Review MariaDB pod status

3. **Plugin Installation Failures**
   - Verify GitHub token permissions
   - Check internet connectivity
   - Review init job logs

## Support

For issues, feature requests, or contributions:

1. Open an issue in the repository
2. Provide relevant logs and configuration
3. Follow the contribution guidelines

## License

This Helm chart is released under the [MIT License](LICENSE).
