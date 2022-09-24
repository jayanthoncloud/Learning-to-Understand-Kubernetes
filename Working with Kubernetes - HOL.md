# Working with Kubernetes - Hands On Lab
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
2. Docker or Hyper-V or Virtual Box must be installed
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

