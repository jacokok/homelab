# POD Volume Restore

```bash
kubectl apply -f pod.yaml

kubectl exec -i longhorn-restorer -n default -- tar -xzf - -C /data < file.tar.gz
```
