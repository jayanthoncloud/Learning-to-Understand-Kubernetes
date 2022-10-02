# Useful Kubectl Commands
Let's look at some of the popular Kubectl commands that we should familiarize

To find the number of nodes in a cluster and the kubernetes version
```
kubectl get nodes
```
![image](https://user-images.githubusercontent.com/49147976/193437006-67925f7d-c093-402d-8759-1746a20c0fdf.png)

To find out what is the flavor and version of Operating System on which the Kubernetes nodes are running?
```
kubectl get nodes -o wide
```
![image](https://user-images.githubusercontent.com/49147976/193437020-3db152ca-e30b-4ab0-aa0e-47cfd9e652fd.png)
To find out how many Pods exist in the default namespace
```
kubectl get pods
```
![image](https://user-images.githubusercontent.com/49147976/193437089-a8dc9eb4-ec43-4fb4-84ba-4b67e072a516.png)
Create a new Pod with say nginx from Docker public image repository and also check the number of pods
```
kubectl run nginx --image=nginx
Kubectl get pods
```
![image](https://user-images.githubusercontent.com/49147976/193437190-632d0dce-f448-418e-a81f-4e111cd7277e.png)
To check the image used to create the new pods
```
kubectl describe pod <name of the pod>
```
![image](https://user-images.githubusercontent.com/49147976/193437318-3e7ff2b2-92cd-4c68-a5c0-aa603a66c613.png)
To find out which node these pods are placed on
```
kubectl get pods -o wide
```
![image](https://user-images.githubusercontent.com/49147976/193437385-2528c683-66a8-4a55-becc-f8b113576b0e.png)
To find out how many containers are part of a particular pod, say webapp (Note: it also shows the status of each containers in the pod and the reason if the container is not successfully created)
```
kubectl describe pod webapp
```
![image](https://user-images.githubusercontent.com/49147976/193437525-6b1c5997-ade5-41f6-aea8-a18ec33629f8.png)
To delete a particular pod
```
kubectl delete pod <name of the pod>
```
![image](https://user-images.githubusercontent.com/49147976/193437767-215a09cb-f666-48f5-9071-f97a93636097.png)
To create a pod with a YAML manifest file
```
kubectl run redis --image=redis123 --dry-run=client -o yaml > redis-definition.yaml
kubectl create -f redis-definition.yaml
kubectl get pods
```
![image](https://user-images.githubusercontent.com/49147976/193437970-3fc9f3ee-2b05-4a81-8d6e-36b34d1ad50b.png)
As you can see the container creation is is error state. Let's try to fix that first by running the **describe** command to know why it failed, and then try to fix it by proving the correct image name using **edit** command.
Note that, this can be fixed by 2 methods. You can edit the yaml definition file using the vi editor or notepad and correct the image name and apply the yaml file. You can also use the edit command to correct the image name.
```
kubectl describe pod redis
vi redis-definition.yaml
kubectl apply -f redis-definition.yaml
```
![image](https://user-images.githubusercontent.com/49147976/193438838-462a3cd3-a16e-4450-a5ac-bd85dc2f712d.png)



