### Assign one of your nodes a new label

    Status=VIP

### Create vip.yaml to spawn four busybox containers which sleep the whole time. Include the nodeSelector entry 
    
    Kind = Pod
    Image = busybox
    Args = sleep 100000
    nodeSelector = Status=VIP
