# How to Build a scalable Application? #
As we saw in the previous example of [building voting application](https://github.com/jayanthyk/Learning-to-Understand-Kubernetes/blob/main/Microservices%20Application.md) using just Pods, it is easier to setup and build a microservices based application. However it is not easily scalable. Just having a single pod to host these microservices can cause application downtime whenever the pod goes down highlighting a single point of failure. Hence deploying these Pods using the Deployment method with replicasets is the ideal solution to address scalability and high availibility challenges. Deployment can also help us perform rolling updates, rollbacks without application downtime. So in this lab let's look at how to deploy a scalable voting application using Kubernetes Deployment.

![image](https://user-images.githubusercontent.com/49147976/195256291-0984d639-032b-46ee-8111-3efff0f57e8c.png)
*Source: Udermy - Kubernetes for Absolute Beginers*                                                                                                                     
Like we did in the previous lab, let's now create 4 deployment definition files.     

**Let's create deployment yaml file for the voting app and name it as "voting-app-deploy.yaml"**
```
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: voting-app-deploy
  labels:
    name: voting-app-deploy
    app: demo-voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: voting-app-pod
      app: demo-voting-app
  template: 
    metadata:
      name: voting-app-pod
      labels:
        name: voting-app-pod
        app: demo-voting-app
    spec:
      containers:
        - name: voting-app 
          image: kodekloud/examplevotingapp_vote:v1
          ports:
            - containerPort: 80
```
**Let's create deployment yaml file for the redis database and name it as "redis-deploy.yaml"**
```
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: redis-deploy
  labels:
    name: redis-deploy
    app: demo-voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: redis-pod
      app: demo-voting-app
  template: 
    metadata:
      name: redis-pod
      labels:
        name: redis-pod
        app: demo-voting-app
    spec:
      containers:
        - name: redis 
          image: redis
          ports:
            - containerPort: 6379
```
**Let's create deployment yaml file for the postgres database and name it as "postgres-deploy.yaml"**
```
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: postgres-deploy
  labels:
    name: postgres-deploy
    app: demo-voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: postgres-pod
      app: demo-voting-app
  template: 
    metadata:
      name: postgres-pod
      labels:
        name: postgres-pod
        app: demo-voting-app
    spec:
      containers:
        - name: postgres  
          image: postgres:9.4
          ports:
            - containerPort: 5432
          env:
            - name: POSTGRES_USER
              value: "postgres"
            - name: POSTGRES_PASSWORD
              value: "postgres"
```
**Let's create deployment yaml file for the worker app and name it as "worker-app-deploy.yaml"**
```
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: worker-app-deploy
  labels:
    name: worker-app-deploy
    app: demo-voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: worker-app-pod
      app: demo-voting-app
  template: 
    metadata:
      name: worker-app-pod
      labels:
        name: worker-app-pod
        app: demo-voting-app
    spec:
      containers:
        - name: worker-app 
          image: kodekloud/examplevotingapp_worker:v1
```
**Let's create deployment yaml file for the results app and name it as "result-app-deploy.yaml"**
```
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: result-app-deploy
  labels:
    name: result-app-deploy
    app: demo-voting-app
spec:
  replicas: 1
  selector:
    matchLabels:
      name: result-app-pod
      app: demo-voting-app
  template: 
    metadata:
      name: result-app-pod
      labels:
        name: result-app-pod
        app: demo-voting-app
    spec:
      containers:
        - name: result-app 
          image: kodekloud/examplevotingapp_result:v1
          ports:
            - containerPort: 80
```
Let's now start creating the deployments and respective services. Before you begin make sure to delete the previously created Pods and any services except for the default kubernetes service.  

**Let's now proceed to create the Voting app deployment and it's service**
```
#First create voting app deployment using the voting-app-deploy.yaml file
kubectl create -f voting-app-deploy.yaml
#Now create the voting app service using voting-app-service.yaml
kubectl create -f voting-app-service.yaml
#Confirm if the Pods and Services are in running state
```
![image](https://user-images.githubusercontent.com/49147976/195285449-b3ad6a14-5494-4e1a-9bbe-4da5b7c7db20.png)

**Let's now proceed to create the redis deployment and it's service**
```
#First create redis deployment using the redis-deploy.yaml file
kubectl create -f redis-deploy.yaml
#Now create the redis service using redis-service.yaml
kubectl create -f redis-service.yaml
#Confirm if the Pods and Services are in running state
```
![image](https://user-images.githubusercontent.com/49147976/195286053-e9469d9b-7413-44a1-b7c5-9a560254ef9c.png)

**Let's now proceed to create the postgres deployment and it's service**
```
#First create postgres deployment using the postgres-deploy.yaml file
kubectl create -f postgres-deploy.yaml
#Now create the postgres service using postgres-service.yaml
kubectl create -f postgres-service.yaml
#Confirm if the Pods and Services are in running state
```
![image](https://user-images.githubusercontent.com/49147976/195296699-0e488d5a-9d9f-4406-947f-a54f42738d68.png)

**Let's now proceed to create the worker app deployment and it's service**
```
#First create worker app deployment using the worker-app-deploy.yaml file
kubectl create -f worker-app-deploy.yaml
#Confirm if the Pod is in running state
```
![image](https://user-images.githubusercontent.com/49147976/195297697-4660444f-9e1f-473e-8c34-f54ac08620b8.png)

**Let's now proceed to create the result app deployment and it's service**
```
#First create result app deployment using the result-app-deploy.yaml file
kubectl create -f result-app-deploy.yaml
#Now create the results service using result-app-service.yaml
kubectl create -f result-app-service.yaml
#Confirm if the Pods and Services are in running state
```
![image](https://user-images.githubusercontent.com/49147976/195298548-af6e0ea4-572b-4bee-a863-62046451ddea.png)
![image](https://user-images.githubusercontent.com/49147976/195298867-d15dd98f-0650-40be-8d87-6bd7cc02b7d0.png)

Let's check the service end point URLs for both voting app and results app and makes ure you're able to acess both URLs from the web browser.

![image](https://user-images.githubusercontent.com/49147976/195299637-4cdadfff-ccd2-42d9-b8f9-f17d60b7cb4e.png)

Let's try to scale the voting app pods to 3
```
kubectl scale deployment voting-app-deploy --replicas=3
#check the deployment and pods
kubectl get deployments voting-app-deploy
#check the pods
kubectl get pods
```
![image](https://user-images.githubusercontent.com/49147976/195301726-683136ca-75a4-4a3e-8cfa-fb4dccf84e77.png)

Note that each time you cast a vote, the page is served by a different pod. That's it with the demo.

