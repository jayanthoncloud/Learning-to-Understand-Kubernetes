# Working with Kubernetes Deployments

## What is Kubernetes Deployment and it's use cases? ##
A Kubernetes Deployment tells Kubernetes how to create or modify instances of the pods that hold a containerized application. Deployments can help to efficiently scale the number of replica pods, enable the rollout of updated code in a controlled manner, or roll back to an earlier deployment version if necessary.

Kubernetes saves time and mitigates errors by automating the work and repetitive manual functions involved in deploying, scaling, and updating applications in production. Since the Kubernetes deployment controller continuously monitors the health of pods and nodes, it can make changes in real-time—like replacing a failed pod or bypassing down nodes—to ensure the continuity of critical applications.

### Let's now learn how to work with Kubernetes Deployments ###

Let's try to create a deployment using the yaml definition file. Pay attention to the objects in the deployment yaml file. This is best created using yaml editor like Virtual Studio Code. Below is the snapshot of a replicaset yaml file used to create nginx pods. Pay attention to the spaces and indentations for the parent and child objects. Copy and pase the below data into yaml file and save the file as "deployment.yaml" Note: The kind is "Deployment"
```
apiVersion: apps/v1
kind: Deployment  
metadata:
  name: myapp-deployment
  labels:
    tier: frontend
    app: nginx
spec:
  selector:
    matchLabels:
      app: myapp
  replicas: 3
  template:     
    metadata:
      name: nginx-2
      labels:
        app: myapp    
    spec:
      containers:
      - name: nginx
        image: nginx
```
Let's now create deployment using the yaml file we created in the above step
```
kubectl create -f deployment.yaml
```
![image](https://user-images.githubusercontent.com/49147976/193578883-aa5d9b73-f819-4486-aaa8-b39c35cb14a4.png)

To know more about the deployment, you can also use "describe" command
```
#kubectl describe deployment <name of the deployment as specified in the yaml file>
kubectl describe deployment myapp-deployment
```
![image](https://user-images.githubusercontent.com/49147976/193579521-036b5e3f-5567-488d-b202-27801362af64.png)

To get information on pods, replicaset, deployment and kubernetes cluster we can use "get all" command
```
#To display all pods, replicasets, deployment we can use "get all" command
kubectl get all
```
![image](https://user-images.githubusercontent.com/49147976/193580771-c5725f0d-5dd3-4b9b-8b45-f0aae9172d6c.png)
