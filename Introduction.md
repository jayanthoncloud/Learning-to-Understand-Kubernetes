# Learning-to-Understand-Kubernetes
As I travel through the journey of trying to understand Kubernetes from **Basics of Kubernetes from Udemy**, I am documenting my learnings on this Github repo. This will be an ongoing repo, as I learn various concepts from the world of Kubernetes. So stay tuned!
## History of Kubernetes
Kubernetes also known as K8s was originally developed by Google and was outsourced in 2014. Kuberenetes is built on 15 years of experience at Google in a project called Borg. Kubernetes is written in Go language, a portable language which is like a hybridization between C++, Python and Java. Instead of deploying big servers, Kubernetes approaches the same by deploying large number of small servers or microservices. The transient nature of smaller microservices also allows for decoupling. Communication between these microservices is API call-driven. Cluster configuration is stored in JSON format, but is most often written in YAML.
## What are Containers?
To understand Kubernetes, we must first understand two things – Container and Orchestration. Once we get familiarized with both of these terms we would be in a position to understand what kubernetes is capable of. We will start looking at each of these next. Before we try and understand what are containers, let's first understand why containers?
### Problem Statement
![image](https://user-images.githubusercontent.com/49147976/191176534-12e5ad84-c884-41d2-874d-29f0c316284c.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

Let's try and decode from the above diagram, what is known as a traditional way of developing applications. There is a need to setup end-end stack including various technologies like web server using g NodeJS and a database such as MongoDB/CouchDB, messaging system like Redis and an orchestration tool like Ansible. When I was a tester/developer, I had lot of difficulty developing this application with all these different components. 

Firstly, their compatibility with the underlying OS. We had to ensure that all these different services were compatible with the version of the OS we were planning to use. There have been times when certain version of these services were not compatible with the OS, and we have had to go back and look for another OS that was compatible with all of these different services. 

Secondly, we had to check the compatibility between these services and the libraries and dependencies on the OS. We have had issues where one service requires one version of a dependent library whereas another service required another version. The architecture of our application changed over time, we have had to upgrade to newer versions of these components, or change the database etc and everytime something changed we had to go through the same process of checking compatibility between these various components and the underlying infrastructure. This compatibility matrix issue is usually referred to as the matrix from hell. 

Next, everytime we had a new developer on board, we found it really difficult to setup a new environment. The new developers had to follow a large set of instructions and run 100s of commands to finally setup their environments. They had to make sure they were using the right Operating System, the right versions of each of these components and each developer had to set all that up by himself each time. 

We also had different development test and production environments. One developer may be comfortable using one OS, and the others may be using another one and so we couldn’t gurantee the application that we were building would run the same way in different environments. And So all of this made our life in developing, building and shipping the application really difficult.

So we needed something that could help us with the compatibility issue. And something that will allow us to modify or change these components without affecting the other components and even modify the underlying operating systems as required. 


### Docker is the answer?
![image](https://user-images.githubusercontent.com/49147976/191179727-e49fe931-0194-4f82-8763-566024a158aa.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

With Docker we were able to run each component in a separate container, with its own libraries and its own dependencies. All on the same VM and the OS, but within separate environments or containers. We just had to build the docker configuration once, and all our developers could now get started with a simple “docker run” command. Irrespective of what underlying OS they run, all they needed to do was to make sure they had Docker installed on their systems.


### Containers

That brings us to, what are containers?

Containers are completely isolated environments, as in they can have their own processes or services, their own network interfaces, their own mounts, just like Virtual machines, except that they all share the same OS kernel. We will look at what that means in a bit. But its also important to note that containers are not new with Docker. Containers have existed for about 10 years now and some of the different types of containers are LXC, LXD , LXCFS etc. Docker utilizes LXC containers. Setting up these container environments is hard as they are very low level and that is were Docker offers a high-level tool with several powerful functionalities making it really easy for end users like us.

## What is Docker?
Before we attempt to understand Docker, let's first try and understand how an Operating System works.

![image](https://user-images.githubusercontent.com/49147976/191182440-c404cd0a-fadd-4fee-bb95-0eb85d365ac4.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

If you look at operating systems like Ubuntu, CentOS, Fedora, Suse etc. they consist of two main things. Kernel and Software. What differentiates these different operating system distributions is the software component although they all share the same kernel. This software may consist of a different User Interface, drivers, compilers, File managers, developer tools etc. So you have a common Linux Kernel shared across all OSes and some custom softwares that differentiate Operating systems from each other.

![image](https://user-images.githubusercontent.com/49147976/191190616-f487e4c3-7080-4d55-b555-e7f40644b95f.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

Sharing the same Kernel? What does that mean?

Let's say Docker is installed on top of Ubuntu OS, Docker can run any flavour of OS on top of it as long as it is the same kernel, in this case Linux. So typically Docker should be able to run containers based on Linux OS distributions like Suse, Debian, Fedora or CentOS. Each docker container only has the additional software, that makes these operating systems different and docker utilizes the underlying kernel of the Docker host which works with all OSes above.

So what is an OS that do not share the same kernel as these? Windows ! And so you wont be able to run a windows based container on a Docker host with Linux OS on it. 
For that you would require docker on a windows server.

You might ask isn’t that a disadvantage then? Not being able to run another kernel on the OS? The answer is No! Because unlike hypervisors, Docker is not meant to 
virtualize and run different Operating systems and kernels on the same hardware. The main purpose of Docker is to containerize applications and to ship them and run 
them. 

### Containers Vs. Virtual Machines

Since most of us are from Virtual Machine background, it's good to understand the difference between Virtual Machines and Containers. 

![image](https://user-images.githubusercontent.com/49147976/191199335-5fc88bc7-a712-4ca0-ae2d-ca7a41ff751a.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

As seen from the above diagram, in case of Docker, we have the underlying hardware and then the OS and then on top Docker manages the containers that run libraries and dependencies alone. In case of Virtual Machines, we have OS on the underlying hardware, then hypervisor like ESX or Hyper-V. As you can see each Virtual Machine runs it's own version of OS inside it and then the application and it's libraries and dependencies

This overhead causes higher utilization of underlying resources as there are multiple virtual operating systems and kernel running. The virtual machines also consume 
higher disk space as each VM is heavy and is usually in Giga Bytes in size, wereas docker containers are lightweight and are usually in Mega Bytes in size.

This allows docker containers to boot up faster, usually in a matter of seconds whereas VMs we know takes minutes to boot up as it needs to bootup the entire OS.

It is also important to note that, Docker has less isolation as more resources are shared between containers like the kernel etc. Whereas VMs have complete isolation from each other. Since VMs don’t rely on the underlying OS or kernel, you can run different types of OS such as linux based or windows based on the same hypervisor.

Note that there are many containerized applications readily available today. Many organizations have containerized applications made available to public by uploading them to Docker public registry called Dockerhub. 

Bringing up a containerized application is as easy as downloading that image and running it using the Docker run command. For example running >Docker run ansible will run an instance of Ansible on Docker host. Likewise, you can bring up and instance of container instances by simply running mongodb, redis, nodejs images.

```
docker run ansible

docker run mongodb

docker run redis

docker run nodejs
```

### Containers Vs Images

![image](https://user-images.githubusercontent.com/49147976/191204280-06f14ecc-83fd-49e7-b113-414ec119159a.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

Now that I mentioned Images in the previous sentence, it is important to understand the difference between Containers and Images. An Image is a package or a template just like a VM template that we refer to in the Virtual Machine world. Containers are running instances off images that are isolated and have their own environments and set of processes.

If you can't find the desired application image in the Dockerhub you can create an image yourself and make it publicly available in Dockerhub.

### Container Advantages

We talked so much about Containers, it's now important to understand what are some of the advantages of Containers. 

If you look at it, traditionally developers developed applications. Then they hand it over to Ops team to deploy and manage it in production environments. They do that by providing a set of instructions such as information about how the hosts must be setup, what pre-requisites are to be installed on the host and how the dependencies are to be configured etc. Since the Ops team did not develop the application on their own, they struggle with setting it up. When they hit an issue, they work with the developers to resolve it. 

With Docker, a major portion of work involved in setting up the infrastructure is now in the hands of the developers in the form of a Docker file. The guide that the developers built previously to setup the infrastructure can now easily put together into a Dockerfile to create an image for their applications. This image can now run on any container platform and is guaranteed to run the same way everywhere. So the Ops team now can simply use the image to deploy the application. Since the image was already working when the developer built it and operations are not modifying it, it continues to work the same when deployed in production.

## Container Orchestration
Alright, we learnt about containers and how applications are packaged inside a container. What if number of applications grow overtime so are users and the dependent application packages such as database, messaging service, web applications etc. This requires the use of more such container to be deployed. You would also need a resource which needs to orchestrate the connectivity between containers and automatically scale up or down based on load. This whole process of automatically deploying and managing these containers at scale is known as **Container Orchestration** 

Now that we understand the importance of a good Container Orchestration technology, let's explore the most popular Container Orchestration technology available today. 

![image](https://user-images.githubusercontent.com/49147976/191417676-e603bbca-db49-4b34-8ac2-07579eb40ee3.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

**Docker Swarm** from Docker

While Docker Swarm is really easy to setup and get started, it lacks some of the advanced autoscaling features required for complex applications.

**Mesos** from Apache

Mesos on the other hand is quite difficult to setup and get started, but supports many advanced features. 

**Kubernetes** from Google

Kubernetes - arguably the most popular of it all – is a bit difficult to setup and get started but provides a lot of options to customize deployments and supports deployment of complex architectures. Kubernetes is now supported on all public cloud service providers like GCP, Azure and AWS and the kubernetes project is one of the top ranked projects in Github.

There are many advantages with Kubernetes. The application is now highly available since there can be multiple instances of application running on different nodes, so even when nodes fail, the application is highly available. When the demand increases, you can deploy more instances of application in minutes and destroy the same when the load discreses to save cost and resources. When we run out of hardware resources, scale the number of nodes up/down without having to take down the application. And do all of these easily with a set of declarative object configuration files. How cool is that?

Ladies and Gentlemen, that is **Kubernetes**. It is a container orchestration technology used to deploy and manage 100s and 1000s of containers at scale in a clustered environment. 

Before we dive into the Lab, it is important to understand the Kubernetes architecture and various terminologies used in the Kubernetes framework.

## Kubernetes Architecture

![image](https://user-images.githubusercontent.com/49147976/191431852-c1a6557b-7a04-4286-b176-0a7a21efb71b.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

**Node**

Node is a machine which can be physical or virtual where containers will be launched by Kubernetes

**Cluster**

A Cluster is a group of nodes on which containerized applications are deployed. If one node fails the application is still accessible from other nodes in the cluster. Having multiple nodes in a cluster not only aids in the application being highly available but also ensures that the application traffic is load balanced across various nodes in the cluster.

**Master**

Now we have a cluster, but who is responsible for managing the cluster? Were is the information about the members of the cluster stored? How are the nodes monitored? When a node fails how do you move the workload of the failed node to another worker node? That’s were the Master comes in. The master is another node with Kubernetes installed in it, and is configured as a Master. The master watches over the nodes in the cluster and is responsible for the actual orchestration of containers on the worker nodes. 

Now it's time to understand various components and their functions in the Kubernetes cluster

![image](https://user-images.githubusercontent.com/49147976/191438716-b5c39da9-e57e-4c12-8af3-6f14f74be9de.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

When you install Kubernetes on a System, you are actually installing the following components. An API Server. An ETCD service. A kubelet service. A Container Runtime, Controllers and Schedulers.

**API Server**

The API server acts as the front-end for kubernetes. The users, management devices, Command line interfaces all talk to the API server to interact with the kubernetes cluster.

**etcd**
ETCD is a distributed reliable key-value store used by kubernetes to store all data used to manage the cluster. Think of it this way, when you have multiple nodes and multiple masters in your cluster, etcd stores all that information on all the nodes in the cluster in a distributed manner. ETCD is responsible for implementing locks within the cluster to ensure there are no conflicts between the Masters. 

**Scheduler**

The scheduler is responsible for distributing work or containers across multiple nodes. It looks for newly created containers and assigns them to Nodes.

**Controller**

Controller is also referred to as the brain behind orchestration. They are responsible for noticing and responding when nodes, containers or endpoints goes down. The controllers makes decisions to bring up new containers in such cases.

**Container runtime**

The container runtime is the underlying software that is used to run containers. In our case it happens to be Docker. 

**Kubelet**

Kubelet is the agent that runs on each node in the cluster. The agent is responsible for making sure that the containers are running on the nodes as expected.

### Master Vs. Worker Node 

![image](https://user-images.githubusercontent.com/49147976/191455254-d4a36e97-69f1-49ba-ae48-e9b8b1aabb94.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

It's important to understand how does a node become a Master vs Worker?

The master server has the kube-apiserver and that is what makes it a master. All the information gathered are stored in a key-value store on the Master. The key-value store is based on the popular etcd framework. The master also has the controller manager and the scheduler.

Similarly the worker nodes have the kubelet agent that is responsible for interacting with the master to provide health information of the worker node and carry out actions requested by the master on the worker nodes. 

### The Famous Kubectl command ###

![image](https://user-images.githubusercontent.com/49147976/191668527-9d6b5d22-bd8b-4566-b9a3-3051b2f950ce.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

The Kubectl tool is used to deploy and manage applications on a kubernetes cluster, to get cluster infromation, get the status of nodes in the cluster and many other cluster operations.

### What is a POD? ###

![image](https://user-images.githubusercontent.com/49147976/192710262-e942a5db-6e16-4c1f-a3e5-6b6396a50905.png)
*Source: Udermy - Kubernetes for Absolute Beginers*

Here we see the simplest of simplest cases were you have a single node kubernetes cluster with a single instance of your application running in a single docker container encapsulated in a POD. What if the number of users accessing your application increase and you need to scale your application? You need to add additional instances of your web application to share the load. Now, where would you spin up additional instances? Do we bring up a new container instance within the same POD? No! We create a new POD altogether with a new instance of the same application. As you can see we now have two instances of our web application running on two separate PODs on the same kubernetes system or node. 

What if the user base FURTHER increases and your current node has no sufficient capacity? Well then you can always deploy additional PODs on a new node in the cluster. You will have a new node added to the cluster to expand the cluster’s physical capacity. So, what I am trying to illustrate in this figure is that, PODs usually have a one-to-one relationship with containers running your application. To scale UP you create new PODs and to scale down you delete PODs. You do not add additional containers to an existing POD to scale your application. 

### What is a YAML file? ###

YAML is used to represent data in an easy to read format. It is similar to XML or JSON which is used to define the structure of data. In YAML, it's important to understand the difference between Lists
