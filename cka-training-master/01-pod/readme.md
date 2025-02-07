## Using Kubectl to create:

- A Pod \
image = `sirfragalot/docker-demo:dcus` \
\
`kubectl run my1stpod --image=sirfragalot/docker-demo:dcus --restart=Never`

- Describe the Pod \
`kubectl describe po my1stpod`

- Get Events \
`kubectl get events`

Access the pod (hint: access the webpage in the container)
- Internal IP \
`kubectl run busybox --image=busybox --restart=Never --rm -ti -- wget -O- http://xxx.xxx.xxx.xxx:8080`

- Port-Forward \
`kubectl port-forward pod my1stpod 8080:8080`

- Use the filter command to show only above pod \
`kubectl get po -l=run=my1stpod`