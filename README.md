# This repository is built as a production-ready DevOps portfolio project.

## Architecture

- Spring Boot running on Kubernetes
- Rancher for cluster administration
- NGINX Ingress to expose services
- ConfigMap / Secret to separate configuration from application code
- PV / PVC on NFS for persistent storage
- MariaDB StatefulSet
- Redis Helm chart
- HPA for automatic scaling
- Prometheus / Grafana / Uptime Kuma for monitoring
- Velero + MinIO for backup / restore
- GitLab CI/CD for build, test, containerization, and deployment

## Folder structure

```text
├─ k8s/
│  ├─ 00-namespace.yaml
│  ├─ 01-resourcequota.yaml
│  ├─ 02-configmap.yaml
│  ├─ 03-secret.yaml
│  ├─ 04-pv.yaml
│  ├─ 05-pvc.yaml
│  ├─ 06-deployment.yaml
│  ├─ 07-service.yaml
│  ├─ 08-ingress.yaml
│  ├─ 09-hpa.yaml
│  ├─ 10-mariadb-statefulset.yaml
│  └─ 11-redis-values.yaml
├─ helm/
│  ├─ ingress-nginx/
│  ├─ monitoring/
│  └─ redis/
├─ monitoring/
│  ├─ kube-prometheus-stack-values.yaml
│  └─ uptime-kuma-values.yaml
├─ backup/
│  ├─ minio-compose.yml
│  └─ velero-notes.md
└─ CI/CD/
   └─ gitlab-ci.yaml
```

## Deployment flow

1. Create the namespace and resource quota.
2. Create PV / PVC on NFS.
3. Create ConfigMap and Secret.
4. Deploy the Spring Boot app with a Deployment.
5. Expose it internally with a Service.
6. Expose it externally with an Ingress.
7. Enable HPA when traffic increases.
8. Use MariaDB StatefulSet for the database.
9. Use Redis Helm chart for caching / HA.
10. Add monitoring and backup.

## CI/CD

The GitLab pipeline performs:
- `test`: run Maven tests
- `package`: build the JAR file
- `docker`: build and push the container image
- `deploy`: apply manifests to Kubernetes

### GitLab variables to create
- `DOCKER_USER`
- `DOCKER_PASSWORD`
- `GITHUB_TOKEN` if you want to sync to GitHub
- `GITHUB_OWNER`
- `GITHUB_REPO`
- `KUBECONFIG_B64` if you want automatic deploy to the cluster

## Notes

- Replace IPs, domains, image names, and passwords with values from your environment.
- `k8s/11-redis-values.yaml` is the Redis Helm values file, aligned with the chart structure in `helm/redis/`.
- `helm/ingress-nginx/`, `helm/monitoring/`, and `helm/redis/` are included so you can extend the project with reusable Helm-based deployments.
