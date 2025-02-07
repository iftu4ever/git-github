### Install software on controller

```bash
sudo apt-get update && sudo apt-get install -y nfs-kernel-server
```

### Make and populate a directory to be shared. Also give it similar permissions to /tmp/

```bash
sudo mkdir /opt/sfw && sudo chmod 1777 /opt/sfw/
sudo bash -c  'echo software > /opt/sfw/hello.txt'
```

### Edit the NFS server file to share out the newly created directory. In this case we will share the directory with all.

```bash
sudo vim /etc/exports
/opt/sfw/ *(rw,sync,no_root_squash,subtree_check)
```

### Cause /etc/exports to be re-read:

```bash
sudo exportfs -ra
```

### Test by mounting the resource from your worker-0 node.

```bash
sudo apt-get -y install nfs-common
sudo mount controller:/opt/sfw /mnt
ls -l /mnt
```
### Install nfs-commons across all your nodes
```bash
sudo apt-get -y install nfs-common
```

### Create a YAML file for the object with 
	kind = PersistentVolume
	name = pvvol-1 
	storage Capacity = 1Gi
	accessModes = ReadWriteMany 
	persistentVolumeReclaimPolicy = Retain
#### NFS Options
    path = /opt/sfw 
	server = controller
### Apply the YAML file to create the PV
### Verify the PV has been created
