### Create a YAML file for the new pvc. 
	kind = PersistentVolumeClaim 
	name = pvc-one 
	storage request = 200Mi 
	accessModes = ReadWriteMany 

### Apply the YAML file to create the PVC
### Verify the PVC has been created
### Check of the PV has been claimed
