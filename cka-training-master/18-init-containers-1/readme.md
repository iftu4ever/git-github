### Create a pod
Use an init container to create a file with the value “hello” into a non-persistent volume at /mnt/hi 

Access a redis container in that pod and mount the same non-persistent volume and confirm the file exists at /mnt/hi
