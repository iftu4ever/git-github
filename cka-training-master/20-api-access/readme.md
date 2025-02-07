Make an API call to retrieve all the namespaces on the cluster. Do not use Kubectl Proxy

```bash
curl --cert kubernetes.pem --key kubernetes-key.pem --cacert ca.pem https://${KUBERNETES_PUBLIC_ADDRESS}:6443//api/v1/namespaces/
```

Make an API call to retrieve all the namespaces on the cluster. Use Kubectl Proxy

```bash
kubectl proxy --port=8080 &
curl http://localhost:8080/api/v1/namespaces/
```