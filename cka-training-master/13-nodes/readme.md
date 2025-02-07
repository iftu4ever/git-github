### Mark a node as unschedulable
``` bash
 kubectl cordon worker-0
 ```
### Drain a node
``` bash
 kubectl drain worker-0
 ```
### Mark node as schedulable
``` bash
 kubectl uncordon worker-0
 ```
### Show metrics for a given node
``` bash
 kubetop node worker-0
 ```
### Taint nodes
``` bash
 kubectl taint nodes worker-0 tainted=true:NoSchedule
 ```
 ### Remove a taint nodes
``` bash
 kubectl taint nodes worker-0 tainted:NoSchedule-
 ```
### Add labels to nodes
``` bash
 kubectl label node worker-0 favorite=true
 ```
### Filter nodes on Labels
``` bash
 kubectl get node -l favorite=true
 ```
### Remove label from nodes
``` bash
 kubectl label node worker-0 favorite-
 ```
