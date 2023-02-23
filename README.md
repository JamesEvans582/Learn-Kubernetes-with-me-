# Learn-Kubernetes-with-me-

In this project we'll learn and explore Kubernetes.

Firstly Installation. You will need to install 3 things. 

.Minikube - minikube quickly sets up a local Kubernetes cluster on macOS, Linux, and Windows (https://minikube.sigs.k8s.io/docs/)

And

.A Container or virtual machine manager, such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation 

And

.Kubectl - The Kubernetes command-line tool, allowing you to run commands against Kubernetes cluster (https://kubernetes.io/docs/tasks/tools/)

You can use these links or terminal - brew install (container) - brew install minikube


In order to start a cluster we need to begin locally. Being a Mac user I have chosen Docker. Which is also a great resource as you can pull images from Docker Hub. 

1) In your terminal type | minikube start --driver=docker

<img width="831" alt="Screenshot 2023-02-23 at 12 55 16" src="https://user-images.githubusercontent.com/71371405/220913350-3238ccc2-7b6f-4374-a96e-a27f1285e470.png">

One thing to note, is that automatically Kubectl is now cofigured to use. 

If you are using Docker the same as me and you have downloaded it to your desktop, you should see this appear.
<img width="1792" alt="Screenshot 2023-02-23 at 12 55 34" src="https://user-images.githubusercontent.com/71371405/220914278-3f91d33f-b6d1-48bd-b8bd-71164ab7129c.png">

Docker on the desktop is visually easy to see whats going on. You can see images, container (if they're active) and logs. 

If you wanted to see this via terminal, you can enter - minikube status

An interesting fact, even if you don't use Docker as your container, within minikube node itself uses docker.  

2) A node is just a server, physical or virtual. In our case it is virtual, and in order to connect we will need SSH. This means we will need to find the IP address of our Kubernetes node.

Simply type - minikube ip 
Then - ssh docker@ insert your ip here (no spaces between @/IP)

<img width="1672" alt="Screenshot 2023-02-23 at 14 25 26" src="https://user-images.githubusercontent.com/71371405/220935460-7e7530c3-03de-4e49-8b3d-82aaa4aa70ee.png">

now you should be inside of your kubernetes node - if you want to leave simply type exit. 
let see what containers are running. Type - docker ps (lists the containers)

<img width="544" alt="Screenshot 2023-02-23 at 14 43 54" src="https://user-images.githubusercontent.com/71371405/220940116-f4a74701-c76a-464a-9697-aaa7d794d2e6.png">

There will be a few containers in there already. Every container has it's purpose - kube-Apiserver / kube-proxy / storage ect, These services will be running on master and worker nodes actually inside the containers. 

Just to note Kubeclt will not work inside of the of the kubernetes node. Kubeclt is an external tool to manage the Kubenetes cluster. So lets leave the ssh connection, type exit in the terminal.

3) Firstly, in your terminal type - Kubectl cluster info 


