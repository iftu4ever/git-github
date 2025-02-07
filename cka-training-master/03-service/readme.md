## Using Kubectl to create a service to expose your deployment from Lab 2

`kubectl expose deploy my1stdeployment --port=8080`

- Access the service internally from the cluster (create a busybox pod) \
`kubectl run -it --rm busybox --image=busybox -- wget my1stdeployment:8080`

- Access the service externally to the cluster \
`kubectl port-forward my1stdeployment-xxxxxx-xxxxxx 8080:8080`

- Describe services \
`kubectl describe svc my1stdeployment`
