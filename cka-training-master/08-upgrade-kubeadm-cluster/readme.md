The upgrade workflow at high level is the following:

- Upgrade the primary control plane node.
- Upgrade additional control plane nodes. (not applicable in our cluster)
- Upgrade worker nodes.

## Upgrade the Control Plane node
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubeadm=1.27.2-00
```

### Verify kubeadm has updated
```
kubeadm version
```

### Drain the control plane node:
```
kubectl drain controller --ignore-daemonsets
```

### Plan the Upgrade
```
sudo kubeadm upgrade plan
```

### Upgrade the Control Plane Node
```
sudo kubeadm upgrade apply v1.27.2
```

### Uncordon the control plane node
```
kubectl uncordon controller
```

### Upgrade `kubelet` and `kubectl`
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubelet=1.27.2-00 kubectl=1.27.2-00
```

### Verify the control plane node is running the desired version of kubernetes
```
kubectl get nodes
```
```
NAME         STATUS   ROLES    AGE   VERSION
controller   Ready    master   24m   v1.27.2
worker-0     Ready    <none>   19m   v1.27.1
worker-1     Ready    <none>   19m   v1.27.1
worker-2     Ready    <none>   19m   v1.27.1
```

## Upgrade the worker nodes
The upgrade procedure on worker nodes should be executed one node at a time or few nodes at a time, without compromising the minimum required capacity for running your workloads.

### Upgrade `kubeadm` on ALL worker nodes
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubeadm=1.27.1-00
```

### Drain the first node (run from controller)
```
kubectl drain worker-0 --ignore-daemonsets --force
```

### Upgrade the `kubelet` configuration
```
sudo kubeadm upgrade node
```

### Upgrade `kubelet` and `kubectl`
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubelet=1.27.2-00 kubectl=1.27.2-00
```

### Verify the version has been upgraded (from the controller)
```
kubectl get nodes
```
```
NAME         STATUS	       	            ROLES    AGE   VERSION
controller   Ready    			        master   27m   v1.27.2
worker-0     Ready,SchedulingDisabled   <none>   22m   v1.27.2
worker-1     Ready    			        <none>   19m   v1.27.1
worker-2     Ready    			        <none>   19m   v1.27.1
```

### Uncordon the node (from the controller)
```
kubectl uncordon worker-0
```

## `Repeat the upgrade steps for the other worker nodes (worker-1 & worker-2); from draining the node through to uncordoning.`

## Verify the status of the cluster
After the kubelet is upgraded on all nodes verify that all nodes are available again by running the following command from anywhere kubectl can access the cluster
```
kubectl get nodes
```
```
NAME         STATUS   ROLES    AGE   VERSION
controller   Ready    master   27m   v1.27.2
worker-0     Ready    <none>   22m   v1.27.2
worker-1     Ready    <none>   22m   v1.27.2
worker-2     Ready    <none>   22m   v1.27.2
```
The `STATUS` column should show `Ready` for all your nodes, and the version number should be updated. 
