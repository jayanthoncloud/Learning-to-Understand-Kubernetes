# Working with Kubernetes - Using a simple cluster using Minikube #
Choosing the tools for our lab to setup Kubernetes. There are several tools available to setup Kubernetes, we can set it up locally on our laptops or VMs using solutions like Minikube and Kubeadm. Let's look at each one of them. For this lab we will use Minikube.

### Minikube ### 

Minikube is used to setup a single instance of Kubernetes in an All-in-one setup

### Kubeadm ###

Kubeadm is a tool used tp cnfigure kubernetes in a multi-node setup

### GCP, Azure and AWS ###

There are hosted solutions available in the cloud environment such as GCP, Azure and AWS to setup Kubernetes

### play-with-k8s.com ###

If you don't have any lab environment, you can simply login to play-with-k8s.com and get your hands on a kubernetes cluster instantly

## Let's now explore working with Minikube ##

### Pre-requisites
1. Kubectl utility must be installed (https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)
2. Docker or Hyper-V or Virtual Box must be installed (In this lab I have used Hyper-V. To enable Hyper-V on your laptop, follow the documentation: https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)
3. Minikube installer (https://minikube.sigs.k8s.io/docs/start/)

Note: For this lab I am using Docker environment on Windows. You can install Docker from this location : https://docs.docker.com/desktop/install/windows-install/

### Let's proceed to install Minikube utility for Windows ###

1. First download and install Minikube installer from PowerShell using the command below. Make sure to launch PowerShell as Administrator
```
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```
2. Add the minikube.exe binary to the PATH using the PowerShell command below
```
   $oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){ `
  [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine) `
}
```

### Starting Minikube on my Windows 10 laptop with Hyper-V as Driver ###
![image](https://user-images.githubusercontent.com/49147976/192564615-3dac84c2-ea73-4108-a4a0-1c5e330705cc.png)

Let's check the status of Minikube

![image](https://user-images.githubusercontent.com/49147976/192567192-204ac89f-ec68-47d5-bb46-56a6098bb628.png)

Let's now execute get nodes command to know the number of nodes running.

![image](https://user-images.githubusercontent.com/49147976/192568110-57654f80-c185-4e90-a207-21ed2d8088a3.png)

Note: Minikube is a single node cluster and it should be in a Ready state

### Deploying Hello Minikube application ###

Copy and paste the below commands into PowerShell

```
kubectl create deployment hello-minikube --image=docker.io/nginx:1.23
kubectl expose deployment hello-minikube --type=NodePort --port=80
```

Now get the status of the Deployment

![image](https://user-images.githubusercontent.com/49147976/192571778-ceb2b669-497a-4425-bc65-d6bf20345806.png)

Let's try to get the URL for the application

![image](https://user-images.githubusercontent.com/49147976/192572663-f165af5b-f1e0-4cce-8db1-aadf2fca4570.png)

Launch the URL of the sample web application. That's it!

![image](https://user-images.githubusercontent.com/49147976/192573173-186ca837-90de-41d4-9696-406988c5f458.png)

### You can now cleanup the deployment by deleting the services and deployment ###

![image](https://user-images.githubusercontent.com/49147976/192575356-b1b82258-3825-4518-abf3-880f21b44642.png)

## Working with Pods ##

Let's try to deploy nginx web server from Docker Container Registry

![image](https://user-images.githubusercontent.com/49147976/192761352-9f65f532-fe06-4647-8ab3-e66ee1b9a660.png)

We can check the status of the Pod using the following command
```
kubectl get pods
```
We can get more information about the Pod using "describe" command
```
kubectl describe pod nginx
```
![image](https://user-images.githubusercontent.com/49147976/192762564-467d0b6f-4adc-46e1-bd82-0e12124d179d.png)

If you want to get information about the Pod and it's internal IP address in a tabular format you can use the below command
```
kubectl get pods -o wide
```
Let's now try to create a Pod with a simple YAML definition file

Open a Notepad++ editor and enter the following data in the notepad file. Pat attention to the spaces between the parent objects and child. This is very important. Make sure all parent objets have same indentation and the same rule applies to child objects as well. Save this file as Pod.yaml
```
apiversion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
    tier: frontend
spec:
  containers:
  - name: nginx
    image: nginx
```

Check the indentation of the Pod.yaml file created using the cat command
```
cat Pod.yaml
```
![image](https://user-images.githubusercontent.com/49147976/192947155-0a5e70c9-0d28-49fc-97e1-da8dd241ac8b.png)

Now apply this Pod.yaml file using the kubectl apply command
```
kubectl apply -f Pod.yaml
```



