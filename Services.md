# Working with Kubernetes Services
## What is a Kuberenetes Service? ##
Kubernetes Services enable communication between various components within and outside of the application. Kubernetes Services helps us connect applications together with other applications or users. For example, our application has groups of PODs running various sections, such as a group for serving front-end load to users, another group running back-end processes, and a third group connecting to an external data source. It is Services that enable connectivity between these groups of PODs. Services enable the front-end application to be made available to users, it helps communication between back-end and front-end PODs, and helps in establishing connectivity to an external data source. Thus services enable loose coupling between microservices in our application.

![image](https://user-images.githubusercontent.com/49147976/194712724-9d9c5a1d-3f9f-453e-9022-3d9c8e241461.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

Let's look at some of the usecases of Services. As we discussed Services is responsible for establishing communication between front-end, back-end PODs, external data source which helps users to access the application. Let's look at various types of Services
1. **NodePort Service**
   ![image](https://user-images.githubusercontent.com/49147976/194713506-110ad1c3-0c73-4fa0-a0a0-1146462a7e5d.png)
   *Source: Udermy - Kubernetes for Absolute Beginers*
   
    If you look at it, there are 3 ports involved. The port on the POD were the actual web server is running is port 80. And it is referred to as the targetPort, because     that is were the service forwards the requests to. The second port is the port on the service itself. It is simply referred to as the port. Remember, these terms are     from the viewpoint of the service. The service is in fact like a virtual server inside the node. Inside the cluster it has its own IP address. And that IP address is     called the Cluster-IP of the service. And finally we have the port on the Node itself which we use to access the web server externally. And that is known as the         NodePort. As you can see it is 30008. That is because NodePorts can only be in a valid range which is from 30000 to 32767.
    ### Let's learn how to work with Kubernetes Services ###
    Let's try to create NodePort Service using the yaml definition file. In our previous Lab [Working with Updates and Rollback](https://github.com/jayanthyk/Learning-to-Understand-Kubernetes/blob/main/Updates%20and%20Rollback.md#lets-now-learn-how-to-work-with-kubernetes-updates-and-rollback) we created a deployment using 6 replicasets and deployed nginix. Let's use the same deployment to now deploy services. Pay attention to the objects in the services yaml file. This is best created using yaml editor like Virtual Studio Code. Below is the snapshot of a services yaml file used to NodePort Service. Pay attention to the spaces and indentations for the parent and child objects. Copy and pase the below data into yaml file and save the file as "service-definition.yaml" Note: The kind is "Service"
```
apiVersion: v1
kind: Service
metadata: 
  name: myapp-service
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30004
  selector: 
    app: myapp
```
Let's run "kubectl get pods" to know the running Pods and then proceed to create services
```
#Let's get the list of pods as deployed in the Updates and Rollback lab
kubectl get pods
#create the service using create command
kubectl create -f service-definition.yaml
#Let's check the service created using get services command
kubectl get services
```
![image](https://user-images.githubusercontent.com/49147976/194860542-219ff61b-8230-4bfc-b790-328c8e214487.png)

As seen from the output of get services command, ClusterIP and NodePort internal IP is listed along with the port number through which the nginx web service can be accessed. So if you know the IP address of the worker node you can directly access nginx webserver using that IP followed by the port number. Since this is running on minikube there is a way to get the URL of the service using the minikune service url command. 
```
#minikube service <name of the service> --url
minikube service myapp-service --url
```
![image](https://user-images.githubusercontent.com/49147976/194862712-f27ec483-2cef-4980-b149-1f8d24436abc.png)

You can now launch the web page by copy pasting the above URL in any web browser
![image](https://user-images.githubusercontent.com/49147976/194862332-d028c061-9aca-4fa2-9469-3f84d646cdd2.png)

    

3. **ClusterIP Service**

     
5. **LoadBalancer Service**
