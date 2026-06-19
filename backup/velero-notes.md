# Velero + MinIO

## MinIO
- Tạo MinIO bằng docker compose
- Dùng làm backend lưu backup của Velero

## Velero install
```bash
wget https://github.com/vmware-tanzu/velero/releases/download/v1.15.0/velero-v1.15.0-linux-amd64.tar.gz
tar -xvf velero-v1.15.0-linux-amd64.tar.gz
sudo mv velero-v1.15.0-linux-amd64/velero /usr/local/bin
```

## Environment variables
```bash
export MINIO_URL="http://your-ip-server-minio:9000"
export MINIO_ACCESS_KEY_ID="your-minio-access-key"
export MINIO_SECRET_KEY_ID="your-minio-secret-key"
export BUCKET="your-bucket-name"
```

## Install command
```bash
velero install   --provider aws   --bucket $BUCKET   --secret-file <(echo -e "[default]\naws_access_key_id=$MINIO_ACCESS_KEY_ID\naws_secret_access_key=$MINIO_SECRET_KEY_ID")   --use-node-agent   --backup-location-config region=minio,s3ForcePathStyle="true",s3Url=$MINIO_URL   --plugins velero/velero-plugin-for-aws:v1.5.0   --namespace velero
```

## Backup / restore
```bash
velero backup create ecommerce-v1 --include-namespace=ecommerce
velero restore create ecommerce-v1 --from-backup ecommerce-v1 --include-namespace=ecommerce
velero schedule create daily-cluster-backup --schedule="0 0 * * *" --include-namespaces '*'
```
