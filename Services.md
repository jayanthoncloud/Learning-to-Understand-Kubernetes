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

3. **ClusterIP Service**
     
5. **LoadBalancer Service**
