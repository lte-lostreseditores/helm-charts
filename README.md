# Helm Charts - Los Tres Editores

Repositorio de charts de Helm para aplicaciones de Los Tres Editores.

## 📦 Charts Disponibles

- **springboot-service**: Chart genérico para servicios Spring Boot con observabilidad y seguridad

## 🚀 Uso

### Agregar el repositorio

```bash
helm repo add lostres https://lostreseditores.github.io/helm-charts --username <USER> --password <TOKEN> --pass-credentials
helm repo update
```

### Instalar un chart

```bash
# Descargar chart
helm pull lostres/springboot-service --version 0.1.6

# Instalar con valores personalizados
helm install mi-app lostres/springboot-service \
  --set image.repository=mi-registry/mi-app \
  --set image.digest=sha256:abc123... \
  --set namespace=lte-gestion-dev \
  --set labels.env=dev
```

## 🔧 Configuración

### Variables de entorno obligatorias

```yaml
image:
  repository: "registry.example.com/mi-app"
  digest: "sha256:abc123..."  # Recomendado para producción
  # tag: "latest"  # Solo si digest está vacío

namespace: "lte-gestion-dev"  # o lte-gestion-prod

labels:
  app.kubernetes.io/name: "mi-app"
  app.kubernetes.io/instance: "mi-app-dev"
  app.kubernetes.io/part-of: "lte-gestion"
  owner: "platform"
  cost-center: "engineering"
  env: "dev"
```

### Recursos obligatorios

```yaml
resources:
  limits:
    cpu: 1000m
    memory: 1Gi
  requests:
    cpu: 500m
    memory: 512Mi
```

## 🔒 Seguridad

- **PodDisruptionBudget**: Configurado por defecto
- **NetworkPolicy**: Deny-all con egress DNS
- **SecurityContext**: Usuario no-root, read-only filesystem
- **Resources**: Límites obligatorios

## 📊 Observabilidad

```yaml
serviceMonitor:
  enabled: true
  port: metrics
  path: /actuator/prometheus
```

## 🏗️ Desarrollo

### Estructura del chart

```
charts/springboot-service/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── _helpers.tpl
    ├── deployment.yaml
    ├── service.yaml
    ├── hpa.yaml
    ├── pdb.yaml
    ├── networkpolicy.yaml
    ├── serviceMonitor.yaml
    └── serviceaccount.yaml
```

### Release

Para crear un release:

```bash
# La versión del paquete sale de charts/springboot-service/Chart.yaml (p. ej. 0.1.6).
git tag chart/springboot-service/v0.1.6
git push origin chart/springboot-service/v0.1.6
```

## 📝 Estándares

- **SemVer**: Versiones empiezan en 0.1.0
- **Namespaces**: lte-gestion-dev, lte-gestion-prod
- **Imágenes**: Referenciadas por digest en producción
- **Secrets**: Via External Secrets Operator desde AWS SSM
- **Labels**: Cost/ownership obligatorios

## 🤝 Contribución

1. Fork el repositorio
2. Crear feature branch
3. Hacer cambios
4. Crear Pull Request

## 📞 Soporte

- **Email**: tecnologia@lostreseditores.com
- **Equipo**: Platform Team
