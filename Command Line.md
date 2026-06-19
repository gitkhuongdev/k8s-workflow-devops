# Commands used in the project

## Docker
- docker build -t <image-name>:v1 .
- docker login
- docker tag <image-name>:v1 <dockerhub-user>/<image-name>:v1
- docker push <dockerhub-user>/<image-name>:v1

## Kubernetes core
- kubectl apply -f k8s/00-namespace.yaml
- kubectl apply -f k8s/01-resourcequota.yaml
- kubectl apply -f k8s/02-configmap.yaml
- kubectl apply -f k8s/03-secret.yaml
- kubectl apply -f k8s/04-pv.yaml
- kubectl apply -f k8s/05-pvc.yaml
- kubectl apply -f k8s/06-deployment.yaml
- kubectl apply -f k8s/07-service.yaml
- kubectl apply -f k8s/08-ingress.yaml
- kubectl apply -f k8s/09-hpa.yaml
- kubectl apply -f k8s/10-mariadb-statefulset.yaml

## Update / rollback
- kubectl scale deployment <name> -n ecommerce --replicas=3
- kubectl set image deployment/<name> <container>=<image>:<tag> -n ecommerce
- kubectl rollout history deployment/<name> -n ecommerce
- kubectl rollout undo deployment/<name> -n ecommerce

## Helm
- helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
- helm repo update
- helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
- helm repo add bitnami https://charts.bitnami.com/bitnami
- helm repo add uptime-kuma https://helm.irsigler.cloud

### Helm install struct
- helm install <RELEASE_NAME> <NAME_CHART> --values <VALUEFILE.yaml> --namespace <NAMESPACE>

## Backup
- velero backup create ecommerce-v1 --include-namespace=ecommerce
- velero restore create ecommerce-v1 --from-backup ecommerce-v1 --include-namespace=ecommerce
- velero schedule create daily-cluster-backup --schedule="0 0 * * *" --include-namespaces '*'
