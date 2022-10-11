# How to build a Simple Microservices based application, Voting App ?
## Application Architecture of Voting App

![image](https://user-images.githubusercontent.com/49147976/195018147-58140e54-d4d9-4a3a-85cb-fe718731f8e8.png)

### Let's look at the various components and technologies used in this application ###
**Voting App:** This provides a front end interface for the users to choose between Cats and Dogs. This is a web application developed in Python
**Results App:** This displays the result of the selection made using the Voting App. This is also a web application written in NodeJS
**In-memory Database:** The selection data from the web application is stored in In-memory database. Redis is used here for such purpose.             
**Worker Node:** The Worker Node fetches the data from In-memory database and stores it in persistent database. This app is written in dotNet.            
**Persisten DB:** The data is stored in the Database called DB. This is using PostgresSQL      

As you can see this voting application is developed using different services, different development tools and multiple differnt development platforms such as Python, NodeJS, dotNet etc. This simple application stack demonstrates how easy it is to setup an application using diverse components using Kubernetes. Let's see how we can deploy this application in Kubernetes.

Before we begin, it's important to plan this entire activity. Our Goal is to deploy the containers, establish the connectivity between various components such as frontend, backend, worker node etc. and then enable external access so the users can access both voting app and results app. Let's look at how to establish connectivity between these various components in the architecture. The voting app written in Python needs to access Redis database and the voting app itself needs to be accessed by external user, in this case voters. The worker node needs to access the Redis database to fetch the in-memory data and also access the PostgresSQL to store the persistent data. The result app needs to access the PostgresSQL database to display the voting result and results app itself needs to be accessed by external users. As you can see every component in the architecture needs to be accessed by an external entity except the worker node. As we learn from the services module, if an application needs to be accessed, we need to deploy the services object for each component. In this case we need to build a services object for voting app, Redis database, PostgresSQL database and results app. Keep in mind that except voting app and reuslts app, all other components are accessed internally only. So they're better of using service type ClusterIP. 

![image](https://user-images.githubusercontent.com/49147976/195031685-14739543-3242-4564-89e8-4a0b83c261c7.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

So just to summarize, we will be deploying 5 Pods in total (voting app, redis, worker node, PostegresSQL and results app), 4 services (voting app, redis, PostgresSQL and results app). We will be using the default container images available in Docker repository. 
### Let's now understand how to work with the lab to build the Voting Application ###
As a first step, we need to create the object definition files in yaml. 5 yaml files for the Pods and 4 yaml files for the service. You can copy paste the contents of the yaml file and name it respectively for the Pods and Services you will be creating in the following section.              

**Let's create yaml file for the voting app Pod and name it as "voting-app-pod.yaml"**
```
apiVersion: v1 
kind: Pod
metadata:
  name: voting-app-pod
  labels:
    name: voting-app-pod
    app: demo-voting-app
spec:
  containers:
    - name: voting-app 
      image: kodecloud/examplevotingapp_vote:v1
      ports:
        - containerPort: 80
```
**Let's Create yaml file for the results app Pod and name it as "result-app-pod.yaml"**
```
apiVersion: v1 
kind: Pod
metadata:
  name: result-app-pod
  labels:
    name: result-app-pod
    app: demo-voting-app
spec:
  containers:
    - name: result-app 
      image: kodecloud/examplevotingapp_result:v1
      ports:
        - containerPort: 80
```
**Let's Create yaml file for the redis database Pod and name it as "redis-pod.yaml"**
```
apiVersion: v1 
kind: Pod
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
**Let's Create yaml file for the postgres database Pod and name it as "postgres-pod.yaml"**
```
apiVersion: v1 
kind: Pod
metadata:
  name: postgres-pod
  labels:
    name: postgres-pod
    app: demo-voting-app
spec:
  containers:
    - name: postgres  
      image: postgres
      ports:
        - containerPort: 5432
      env:
        - name: POSTGRES_USER
          value: "postgres"
        - name: POSTGRES_PASSWORD
          value: "postgres"
```
**Let's Create yaml file for the worker app Pod and name it as "worker-app-pod.yaml"**
```
apiVersion: v1 
kind: Pod
metadata:
  name: worker-app-pod
  labels:
    name: worker-app-pod
    app: demo-voting-app
spec:
  containers:
    - name: worker-app 
      image: kodecloud/examplevotingapp_worker:v1
```
Now that we have created yaml files for the 5 Pods, let's now create 4 yaml files for the services.

**Let's Create yaml file for the redis service and name it as "redis-service.yaml"**
```
apiVersion: v1 
kind: Service
metadata:
  name: redis
  labels:
    name: redis-service
    app: demo-voting-app
spec:
  ports:
    - port: 6379
      targetPort: 6379
  selector:
    name: redis-pod
    app: demo-voting-app 
```
**Let's Create yaml file for the postgres service and name it as "postgres-service.yaml"**
```
apiVersion: v1 
kind: Service
metadata:
  name: db
  labels:
    name: postgres-service
    app: demo-voting-app
spec:
  ports:
    - port: 5432
      targetPort: 5432
  selector:
    name: postgres-pod
    app: demo-voting-app 
```
**Let's Create yaml file for the voting app service and name it as "voting-app-service.yaml"**
```
apiVersion: v1 
kind: Service
metadata:
  name: voting-service
  labels:
    name: voting-service
    app: demo-voting-app
spec: 
  type: NodePort 
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30004
  selector:
    name: voting-app-pod
    app: demo-voting-app 
```
**Let's Create yaml file for the results app service and name it as "result-app-service.yaml"**
```
apiVersion: v1 
kind: Service
metadata:
  name: result-service
  labels:
    name: result-service
    app: demo-voting-app
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30005
  selector:
    name: result-app-pod
    app: demo-voting-app 
```
