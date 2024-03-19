# Working with Kubernetes Updates and Rollback

## What is Kubernetes Updates? ##

Let's say Kubernetes deployment deploys a particular instance of your application and tomorrow there is a newer version of the application made available. One needs to update the application instance to the newer version to take advantage of the latest and greatest features. Now there are two ways to do this and this is called ** Deployment Strategy **.

### Deployment Strategy ###

There are two types of deployment strategies. 
1. **Recreate Strategy** Using this strategy destroy the application instances of the replicaset and redeploy with the newer version of the application instances. The problem with this strategy is that since all replicasets have to be destroyed, there is application downtime
2. **Rolling Update Strategy** Using this approach we do not destroy all instances of the replicaset at once, instead we destroy one at a time and bring up a newer updated version of the application instance. This is called rolling update hence there is no application downtime. Note: Rolling update is the default deployment strategy unless otherwise specified. 

## What is Kubernetes Rollback? ##

When you deploy using the default deployment strategy and update the application instances, Kubernetes maintains an older version of the replicaset without completely destroying it. At any point in time if the application instance has to be rolled back to an older version Kubernetes administrator can trigger a rollback operation which changes the version of the application instance to the older version. This process is called **Rollback**

### Let's now learn how to work with Kubernetes Updates and Rollback ###

Make sure there is no pods deployed. Now let's create the deployment using the same yaml file deployment.yaml that we created in the **Deployment** lab. Only change we need to make is update the replicas from 3 to 6. Below the content of deployment.yaml definition file.
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
  replicas: 6
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
Create deployment using the above deployment.yaml file. Observe the rollout deployment status
```
kubectl create -f deployment.yaml
#kubectl rollout status deployment <name of the deployment>
kubectl rollout status deployment myapp-deployment
```
![image](https://user-images.githubusercontent.com/49147976/193994596-ea65f86d-6dfc-4545-be45-3f9348f66977.png)

Let's now check the deployment rollout status as soon as we create the deployment. For this we need to delete the deployment and recreate the deployment and imemdiate issue the rollout status command.
![image](https://user-images.githubusercontent.com/49147976/193995964-a7b500c8-2a07-4b9d-8602-3cfc84a41b00.png)

Now let's check the history of the deployment rollout using **history** command.
![image](https://user-images.githubusercontent.com/49147976/193996437-4b2952ea-0c2f-479f-8502-1b24a8ec1639.png)

Note that Revision is 1 and CHANGE-CAUSE is none. Let's try to fix that using the **record** switch while creating the deployment and observe the cause of change

```
#use the record command to reflect the change-cause parameter in rollout history
kubectl delete deployment myapp-deployment
kubectl create -f deployment.yaml --record
```
![image](https://user-images.githubusercontent.com/49147976/193997353-cb79322d-39f6-4ca0-b4f3-0d773cb337e9.png)

Let's now try to update the version of the nginx to an older version. You can pick the desired version from docker webpage. https://hub.docker.com/_/nginx

For this demo let's update the nginx version to 1.22 using the **edit** command.
```
#use the record option to record the cause of change
kubectl edit deployment myapp-deployment --record
#look for -image section and add 1.22 next to nginx. "-image: nginx:1.22" and save the file
#Run the rollout history command to check the recorded changes
kubectl rollout history deployment myapp-deployment
#Run the describe command for the deployment and check the events and the nginx image version to be updated with 1.22
kubectl describe deployment myapp-deployemnt 
```
![image](https://user-images.githubusercontent.com/49147976/194000377-6b553434-98a4-4c3b-96bd-aee70116e1ef.png)

Pay attention to the events section and verify how Kubernetes orchestrated the update one pod at a time by bringing down the pod with version 1.23 and bringing up a pod with 1.22.

Another method to update the image is to use **set image** command. Let's try to change the version of nginx from 1.22 to 1.22-perl using this command
```
#Just like in the previous steps let's record the cause of change
kubectl set image deployment myapp-deployment nginx=nginx:1.22-perl --record
```
![image](https://user-images.githubusercontent.com/49147976/194003655-92de9fe3-52de-4816-8225-ab9cc49633e4.png)

Pay attention to the events section and verify how Kubernetes orchestrated the update one pod at a time by bringing down the pod with version 1.22 and bringing up a pod with 1.22-perl.

Let's say there is a issue with the current version and we want to rollback to an earler version, we can use the **rollout undo** command to do that. Let's try doing just that.
```
kubectl rollout undo deployment myapp-deployment 
#immediately check the status of the rollout
kubectl rollout status deployment myapp-deployment
#use the rollout history command to check the history
kubectl rollout history deployment myapp-deployment
#use the describe command to verify the image is rolled back to 1.22
kubectl describe deployment myapp-deployment
#check the running pods to ensure all 6 replicas are running
kubectl get pods
```
![image](https://user-images.githubusercontent.com/49147976/194005868-c713b4c1-0310-4a53-b6dc-d6cc85bec4e9.png)

Now in the next exercise let's try to simulate a failure by providing an image that doesn't exist in docker hub and let's see what Kubernetes carries out the update process. 
```
#let's edit the image using the edit command and specify a wrong image name say "-image: nginx:nginx-1.22-does-not-exist"
kubectl edit deployment myapp-deployment --record
#let's check the rollout status, observe the deployment is stalled
kubectl rollout status deployment myapp-deployment
#get the get deployment to understand the pods running
kubectl get deployment myapp-deployment
#Run get pods to know the status of the pods
kubectl get pods
#Also run the rollout histrory command 
kubectl rollout history deployment myapp-deployment
#let's now try to fix this by undoing the rollout
kubectl rollout undo deployment myapp-deployment
#now run the get pods to get the status of the running pods
kubectl get pods
#run the describe command to check the version of the image
kubectl describe deployment myapp-deployment
```
![image](https://user-images.githubusercontent.com/49147976/194013633-cd66cfe0-7d7d-4350-8fa6-636c48d219c1.png)

That's it for this lab.
