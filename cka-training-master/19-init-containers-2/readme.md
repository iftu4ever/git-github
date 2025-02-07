### Create a pod
Use an init container to create a file with the value “hello” into a hostPath volume at /data/hi on the host

Access a redis container in that pod and mount the same hostPath volume and confirm the file exists at /mnt/hi
