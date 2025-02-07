### Install `ectdctl` tool
```
sudo apt-get install -y etcd-client
```

### Verify Certificate locations
```
sudo cat /etc/kubernetes/manifests/etcd.yaml
...
    - --advertise-client-urls=https://10.140.0.10:2379
    - --cert-file=/etc/kubernetes/pki/etcd/server.crt
    - --key-file=/etc/kubernetes/pki/etcd/server.key
    - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
...
```
### Perform the snapshot
```
sudo ETCDCTL_API=3 etcdctl --endpoints https://10.140.0.10:2379 --cacert="/etc/kubernetes/pki/etcd/ca.crt" --cert="/etc/kubernetes/pki/etcd/server.crt" --key="/etc/kubernetes/pki/etcd/server.key" snapshot save snapshotdb
```
### Verify the snapshot
```
ETCDCTL_API=3 etcdctl --write-out=table snapshot status snapshotdb
+----------+----------+------------+------------+
|   HASH   | REVISION | TOTAL KEYS | TOTAL SIZE |
+----------+----------+------------+------------+
| fe01cf57 |       10 |          7 | 2.1 MB     |
+----------+----------+------------+------------+
```
