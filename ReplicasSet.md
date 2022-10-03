# Working with ReplicaSet
## What is a ReplicaSet? ##

A ReplicaSet (RS) is a Kubernetes object that ensures there is always a stable set of running pods for a specific workload. The ReplicaSet configuration defines a number of identical pods required, and if a pod is evicted or fails, creates more pods to compensate for the loss.

Let's try to create a ReplicaSet using the yaml definition file. Pay attention to the objects in the replicaset yaml file. This is best created using yaml editor like Virtual Studio Code. Below is the snapshot of a replicaset yaml file used to create nginx pods. Pay attention to the spaces and indentations for the parent and child objects. Copy and pase the below data into yaml file and save the file as "replicaset.yaml"
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
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
Create replicaset using the yaml file created above
```
#create the replicaset
kubectl create -f replicaset.yaml
#get the replicaset using the get command
kubectl get replicaset
#verify the replica pods created
kubectl get pods
```
![image](https://user-images.githubusercontent.com/49147976/193514731-eeb8a1e6-1a58-4788-b0fb-4e22441979ce.png)

Now, let's try to delete one of the pods in the replicaset and see how the replicaset immediately creates and brings up a new pod
```
kubectl delete pod <name of one of the pods>
```
![image](https://user-images.githubusercontent.com/49147976/193515775-c4725beb-6bd6-4fcf-adf8-71e8f2e4159a.png)

We can also see the replicaset events using the describe command
```
kubectl describe replicaset <name of the replicaset as specified in the yaml file above>
```
![image](https://user-images.githubusercontent.com/49147976/193516570-e3307ea2-26cb-4309-8252-99dca36feec2.png)

Let's now try to create another pod using the yaml definition file, this time without using replicaset object, but using the same label name "myapp" and see what happens. Copy and paste the content and create a yaml definition file and save it as nginx.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-2
  labels:
    app: myapp    
spec:
  containers:
   - name: nginx
     image: nginx
```
Create another pod using the nginx.yaml file created above
```
kubectl create -f nginx.yaml
```
![image](https://user-images.githubusercontent.com/49147976/193519024-38c48079-69dc-428e-a91d-f417d665c7e9.png)

As we see from the screenshot above, as soon as a new pod with the same label is created, replicaset terminates the new pod and eventually deletes it. So replicaset doesn't allow more than 3 pods with the same label to exist in the replicaset.

Now let's see how to scale up or down replicas in a replicaset. This can be done using 2 options
1. Editing the yaml file using the "edit" command
2. Use the command "scale" to scale the replicas up or down.
```
#scaling the replicaset from 3 to 4
kubectl edit replicaset <name of the replicaset>
```
![image](https://user-images.githubusercontent.com/49147976/193521206-293c215c-1583-4efd-97bf-7cea5ce46844.png)

Observe the new pod created. Note that it opens a respective editor and the the config file has lot more information than the yaml file we created. This is a config file that kubernetes creates in memory and be careful not to change any other parameters except the replicaset from 3 to 4 under spec section.

```
#use the scale command to scale down the replicaset from 4 to 2
kubectl scale replicaset <name of the replicaset> --replicas=2
```
![image](https://user-images.githubusercontent.com/49147976/193522384-e3fa5321-7d21-4c42-b66d-2771d9e7d67c.png)


