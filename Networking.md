# Working with Kubernetes Networking
## What are the various network communications we need to understand in Kubernetes?
1. Container to Container Communication
2. Pod to Pod Communication
3. Pod to Service Communication
4. External to Internal Communication

Let's try to understand each of them briefly. 

### Container to Container Communication ###
A Pod possesses a group of containers within a network namespace. These containers have the same port space and IP address, which is assigned to them through the network namespace. These containers find each other through the localhost because they are located in the same namespace. If your applications are within a pod, they can access the shared volumes as well. 
### Pod to Pod Communication ###
We need to consider two scenarios here. Pods communicating within the same node and Pods commuincating between 2 different nodes. 
1. **Pod to Pod communication within the same Node**
   
   When Kubernetes is intially configured there is an internal private network created and each Pods in the Node gets a private IP from this range which enables the Pods    within the same node communicate with each other.
   ![image](https://user-images.githubusercontent.com/49147976/194689873-f69f3238-9775-4dbf-ad1f-d97b0402d887.png)
   *Source: Udermy - Kubernetes for Absolute Beginers*
2. **Pod to Pod Communication between 2 Nodes**

   It is important to note that when Kubernetes is configured it doesn't inherently take care of networking. It expects us to handle the networking. This becomes a          challenge when Pods are trying to communicate with one another on two different nodes. Thankfully this is handled by many of the Kubernetes networking plug-ins such      cisco ACI networks, Cilium, Big Cloud Fabric, Flannel, Vmware NSX-t, Calico etc. All the routing and assignment of IP addresses to various Pods on different nodes are    all handled by these networking solutions. 
   ![image](https://user-images.githubusercontent.com/49147976/194690370-daa1f63e-6699-4001-9c90-74226120edd2.png)
   *Source: Udermy - Kubernetes for Absolute Beginers*


