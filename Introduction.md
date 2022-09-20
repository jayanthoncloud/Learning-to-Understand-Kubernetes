# Learning-to-Understand-Kubernetes
I am using this Github repo to document my journey with Kubernetes, as I prpare myself to crack CKA certification.
## History of Kubernetes
Kubernetes also known as K8s was originally developed by Google and was outsourced in 2014. Kuberenetes is built on 15 years of experience at Google in a project called Borg. Kubernetes is written in Go language, a portable language which is like a hybridization between C++, Python and Java. Instead of deploying big servers, Kubernetes approaches the same by deploying large number of small servers or microservices. The transient nature of smaller microservices also allows for decoupling. Communication between these microservices is API call-driven. Cluster configuration is stored in JSON format, but is most often written in YAML.
## What are Containers?
To understand Kubernetes, we must first understand two things – Container and Orchestration. Once we get familiarized with both of these terms we would be in a position to understand what kubernetes is capable of. We will start looking at each of these next. Before we try and understand what are containers, let's first understand why containers?
### Problem Statement
![image](https://user-images.githubusercontent.com/49147976/191176534-12e5ad84-c884-41d2-874d-29f0c316284c.png)
Let's try and decode from the above diagram, what is known as a traditional way of developing applications. There is a need to setup end-end stack including various technologies like web server using g NodeJS and a database such as MongoDB/CouchDB, messaging system like Redis and an orchestration tool like Ansible. When I was a tester/developer, I had lot of difficulty developing this application with all these different components. 

Firstly, their compatibility with the underlying OS. We had to ensure that all these different services were compatible with the version of the OS we were planning to use. There have been times when certain version of these services were not compatible with the OS, and we have had to go back and look for another OS that was compatible with all of these different services. 

Secondly, we had to check the compatibility between these services and the libraries and dependencies on the OS. We have had issues where one service requires one version of a dependent library whereas another service required another version. The architecture of our application changed over time, we have had to upgrade to newer versions of these components, or change the database etc and everytime something changed we had to go through the same process of checking compatibility between these various components and the underlying infrastructure. This compatibility matrix issue is usually referred to as the matrix from hell. 

Next, everytime we had a new developer on board, we found it really difficult to setup a new environment. The new developers had to follow a large set of instructions and run 100s of commands to finally setup their environments. They had to make sure they were using the right Operating System, the right versions of each of these components and each developer had to set all that up by himself each time. 

We also had different development test and production environments. One developer may be comfortable using one OS, and the others may be using another one and so we couldn’t gurantee the application that we were building would run the same way in different environments. And So all of this made our life in developing, building and shipping the application really difficult.

So we needed something that could help us with the compatibility issue. And something that will allow us to modify or change these components without affecting the other components and even modify the underlying operating systems as required. 


### Docker is the answer?
![image](https://user-images.githubusercontent.com/49147976/191179727-e49fe931-0194-4f82-8763-566024a158aa.png)

With Docker we were able to run each component in a separate container, with its own libraries and its own dependencies. All on the same VM and the OS, but within separate environments or containers. We just had to build the docker configuration once, and all our developers could now get started with a simple “docker run” command. Irrespective of what underlying OS they run, all they needed to do was to make sure they had Docker installed on their systems.


### Containers

That brings us to, what are containers?

Containers are completely isolated environments, as in they can have their own processes or services, their own network interfaces, their own mounts, just like Virtual machines, except that they all share the same OS kernel. We will look at what that means in a bit. But its also important to note that containers are not new with Docker. Containers have existed for about 10 years now and some of the different types of containers are LXC, LXD , LXCFS etc. Docker utilizes LXC containers. Setting up these container environments is hard as they are very low level and that is were Docker offers a high-level tool with several powerful functionalities making it really easy for end users like us.

## What is Docker?
Before we attempt to understand Docker, let's first try and understand how an Operating System works.

![image](https://user-images.githubusercontent.com/49147976/191182440-c404cd0a-fadd-4fee-bb95-0eb85d365ac4.png)

If you look at operating systems like Ubuntu, CentOS, Fedora, Suse etc. they consist of two main things. Kernel and Software. What differentiates these different operating system distributions is the software component although they all share the same kernel. This software may consist of a different User Interface, drivers, compilers, File managers, developer tools etc. So you have a common Linux Kernel shared across all OSes and some custom softwares that differentiate Operating systems from each other.

![image](https://user-images.githubusercontent.com/49147976/191190616-f487e4c3-7080-4d55-b555-e7f40644b95f.png)

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

As seen from the above diagram, in case of Docker, we have the underlying hardware and then the OS and then on top Docker manages the containers that run libraries and dependencies alone. In case of Virtual Machines, we have OS on the underlying hardware, then hypervisor like ESX or Hyper-V. As you can see each Virtual Machine runs it's own version of OS inside it and then the application and it's libraries and dependencies

This overhead causes higher utilization of underlying resources as there are multiple virtual operating systems and kernel running. The virtual machines also consume 
higher disk space as each VM is heavy and is usually in Giga Bytes in size, wereas docker containers are lightweight and are usually in Mega Bytes in size.

This allows docker containers to boot up faster, usually in a matter of seconds whereas VMs we know takes minutes to boot up as it needs to bootup the entire OS.

It is also important to note that, Docker has less isolation as more resources are shared between containers like the kernel etc. Whereas VMs have complete isolation from each other. Since VMs don’t rely on the underlying OS or kernel, you can run different types of OS such as linux based or windows based on the same hypervisor.

Note that there are many containerized applications readily available today. Many organizations have containerized applications made available to public by uploading them to Docker public registry called Dockerhub. 

Bringing up a containerized application is as easy as downloading that image and running it using the Docker run command. For example running >Docker run ansible will run an instance of Ansible on Docker host. Likewise, you can bring up and instance of container instances by simply running mongodb, redis, nodejs images.

>docker run ansible

>docker run mongodb

>docker run redis

>docker run nodejs

### Containers Vs Images

![image](https://user-images.githubusercontent.com/49147976/191204280-06f14ecc-83fd-49e7-b113-414ec119159a.png)

Now that I mentioned Images in the previous sentence, it is important to understand the difference between Containers and Images. An Image is a package or a template just like a VM template that we refer to in the Virtual Machine world. Containers are running instances off images that are isolated and have their own environments and set of processes.

If you can't find the desired application image in the Dockerhub you can create an image yourself and make it publicly available in Dockerhub.

### Container Advantages

We talked so much about Containers, it's now important to understand what are some of the advantages of Containers. 

If you look at it, traditionally developers developed applications. Then they hand it over to Ops team to deploy and manage it in production environments. They do that by providing a set of instructions such as information about how the hosts must be setup, what pre-requisites are to be installed on the host and how the dependencies are to be configured etc. Since the Ops team did not develop the application on their own, they struggle with setting it up. When they hit an issue, they work with the developers to resolve it. 

With Docker, a major portion of work involved in setting up the infrastructure is now in the hands of the developers in the form of a Docker file. The guide that the developers built previously to setup the infrastructure can now easily put together into a Dockerfile to create an image for their applications. This image can now run on any container platform and is guaranteed to run the same way everywhere. So the Ops team now can simply use the image to deploy the application. Since the image was already working when the developer built it and operations are not modifying it, it continues to work the same when deployed in production.

## Container Orchestration
