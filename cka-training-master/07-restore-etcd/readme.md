# etcd-backup-and-restore

## First create a deployment - to use as validation post-restore process (if you are starting on a fresh cluster)
```
kubectl create deploy nginx --image=nginx --replicas=4
```

## Check the deployment is created and the pods are running
```
kubectl get po
NAME                     READY   STATUS    RESTARTS   AGE
nginx-6799fc88d8-54m9f   1/1     Running   0          41s
nginx-6799fc88d8-9mwvv   1/1     Running   0          41s
nginx-6799fc88d8-kghxw   1/1     Running   0          41s
nginx-6799fc88d8-x7d9b   1/1     Running   0          41s
```

## Make backup folder for manifest files
```
mkdir ~/backup
```
## Stop control plane components apart from etcd
```
sudo mv /etc/kubernetes/manifests/kube-apiserver.yaml ~/backup/
sudo mv /etc/kubernetes/manifests/kube-controller-manager.yaml ~/backup/
sudo mv /etc/kubernetes/manifests/kube-scheduler.yaml ~/backup/
```
## Stop kubelet service on all nodes
```
sudo systemctl stop kubelet
```
## Check all components have stopped
Use the following commands to verify the control plane components and kubelet have indeed stopped running. Only etcd should now be running on the controller.
```
ps -ef | grep kube
sudo crictl ps
```
## Perform etcd snapshot backup
```
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key snapshot save snapshotdb
```
## Restart control plane
Remember to restart the kubelet on ALL nodes.
```
sudo mv ~/backup/*.yaml /etc/kubernetes/manifests
sudo systemctl start kubelet
```
## Delete nginx deployment
```
kubectl delete deploy nginx
```
Now wait for all pods to disappear before continuing.
## Stop ALL control plane components
```
sudo mv /etc/kubernetes/manifests/*.yaml ~/backup/
```
Wait for the 4 processes to completely stop.

Next stop the kublet on ALL nodes.
```
sudo systemctl stop kubelet
```
## Expand etcd snapshot file
```
sudo ETCDCTL_API=3 etcdctl snapshot restore snapshotdb_nginx_5_replicas --skip-hash-check=true
```
## Backup existing etcd data folder
```
sudo mv /var/lib/etcd/member ~/backup
```
## Copy the expanded etcd snapshot file
```
sudo cp -r default.etcd/member /var/lib/etcd
```
## Restart control plane
Remember to restart the kubelet on ALL nodes.
```
sudo mv ~/backup/*.yaml /etc/kubernetes/manifests
sudo systemctl start kubelet
```
## Verify the restore is successful
```
kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-6799fc88d8-54m9f   1/1     Running   0          7m48s
nginx-6799fc88d8-9mwvv   1/1     Running   0          7m48s
nginx-6799fc88d8-kghxw   1/1     Running   0          7m48s
nginx-6799fc88d8-x7d9b   1/1     Running   0          7m48s
```

## DONE :-)
