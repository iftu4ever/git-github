## Using Kubectl to create a namespace {yourname} and deploy the below into that ns
 - type = `deployment`
 - image = `sirfragalot/docker-demo:dcus`
 - replicas = `3`
 - Service Type = `NodePort`

##### Namespace:
`kubectl create ns my1stnamespace`

##### Deployment:
`kubectl create deploy my1stdeployment --image=sirfragalot/docker-demo:dcus --namespace=my1stnamespace`

##### Scale to 3 replicas
`kubectl scale deploy my1stdeployment -n my1stnamespace --replicas 3`

##### Service:
`kubectl expose deploy my1stdeployment -ns my1stnamespace --port=8080 --type=NodePort`