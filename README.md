# Minio back up to NFS on Kubernetes

This Minio backup solution on Kubernetes spawns one `mc mirror --watch` pod for each Minio bucket being backed up. Each pod will mirror the target bucket to `/mnt/target/BUCKETNAME` where `/mnt/target` is an external NFS. The pods will synchronize the mirror upon start-up and then listen for changes.

## Configuration

### Namespace

It is recommended to create a new namespace for this.

### Secret

Create a secret using the following template, containing your Minio access key ID and secret key:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: minio-backup
type: Opaque
data:
  accessKeyId: <BASE64 encoded access key ID>
  secretAccessKey: <BASE64 encoded secret access key>
```

### Vars file

Use `default.vars.yaml` as reference and `production.vars.yaml` as example.

## Deployment

Uses [Emskaffolden](https://github.com/con2/emskaffolden) for deployment.

    emsk -E production -- run -n minio-backup
