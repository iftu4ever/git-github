## Using Kubectl to create:
- A deployment of ONE pod, image=`sirfragalot/docker-demo:dcus`\
`kubectl create deploy my1stdeployment --image=sirfragalot/docker-demo:dcus`

## Scaling:
- Scale up to 5\
`kubectl scale deploy my1stdeployment --replicas=5`
- Scale down to 1\
`kubectl scale deploy my1stdeployment --replicas=1`

## Updates:
- Rolling update to image nginx instead – track changes\
`kubectl edit deploy my1stdeployment --record`

- Roll back to previous image\
`kubectl rollout undo deploy my1stdeployment`

- View deployment history\
`kubectl describe deploy my1stdeployment`