# k8s-demo
To run this demo, first start minicube (note the port command.. see the following for more info:https://stackoverflow.com/questions/71384252/cannot-access-deployed-services-when-minikube-cluster-is-installed-in-wsl2)
```
minikube start --ports 127.0.0.1:30100:30100
```

## BUILDING DOCKER CONTAINERS
When building a container, the following command must be run after the minikube is up and before running the docker build command:
```
eval $(minikube docker-env)
```
This is necessary to push the docker images to minikube. see https://stackoverflow.com/questions/52310599/what-does-minikube-docker-env-mean

## Adding pods to minikube

Then, run the following commands to apply the various yaml files:
```
kubectl apply -f mongo-config.yaml
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo.yaml
kubectl apply -f webapp.yaml
```

## viewing the status of the kube
This command will show all the pods that are running
```
kubectl get all
```
This command will show the configmap and secrets
```
kubectl get configmap
kubectl get secret
```
This commands will show the status of the services:
```
kubectl describe service webapp-service
```
..and the staus of a pod
```
kubectl describe pod webapp-deployment-56dbf5d695-g7nm4
```
(note these names came from "kubectl get all")

here's a command to list all the pods
```
kubectl get pod
```
now, how to access our app from the browser?? Need to configure the service
```
kubectl get svc
```
need the IP address of the cluster node, use
```
minikube ip
```
or using k8s
```
kubectl get node -o wide
```
which will give us the IP address of the k8s cluster.

then we should be able to access the web service in the browser. for example 192.168.48.2:30100

## Logs
To view the logs, get the container ID  using this command...
```
kubectl get pod
```
...and use the following command
```
kubectl logs webapi-deployment-b8985dcb9-vj42r -f
```

Sidenote: I couldn't access it using wsl2. Looks like that's a known issue: https://stackoverflow.com/questions/71384252/cannot-access-deployed-services-when-minikube-cluster-is-installed-in-wsl2
I updated the initial minikube start command to include the public facing port that the web server will use.



If you can't access the NodePort service webapp with MinikubeIP:NodePort, execute the following command:
```
minikube service webapp-service
```
