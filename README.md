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
<img width="744" alt="Screenshot 2023-02-23 at 16 47 51" src="https://user-images.githubusercontent.com/71371405/220974558-d304cd5d-9d27-4c7e-9512-4447397a723e.png">

As you can see the CoreDNS service is running as well, which means we are able to create deployments, services ect on our kubernetes cluster.

<img width="414" alt="Screenshot 2023-02-23 at 16 55 39" src="https://user-images.githubusercontent.com/71371405/220976348-d77f9801-b573-4dce-b958-e403b893fdf2.png">

We can now check what nodes are ready to use, type -> kubectl get nodes (this will be working as master and worker)
And we can also check which pods are availible in this cluster, type -> kubectl get pods 
lets list all the namespaces -> kubectl get namespaces (namespaces are used to group different resources and configuration objects)

<img width="384" alt="Screenshot 2023-02-23 at 17 08 07" src="https://user-images.githubusercontent.com/71371405/220979492-fe55bf9a-c0df-4aa3-bd3b-357ca21734e7.png">

So currently these pods are running, if we wanted to list whats running inside of these pods, so systems for example, we can type -> kubectl get pods --namespace=kube-system

4) This is where it get interesting. The Kubectl commands are similar to docker commands. 
When we create our own pod, we specify the pod and pull an Image from Docker Hub. Example underneath.

Kubeclt run nginx --image=nginx  (run nginx is the pod and the image is nginx, but the image doesn't have to match)

The image will be in container - container will be in a pod.

So if you're following along, to run a pod, type -> Kubeclt run nginx --image=nginx 
Or better yet, lets create a deployment, type -> kubectl create deployment nginx-depl --image=nginx (after create deployment nginx- will be the name of the deployment)

Then type-> kubectl get pods

<img width="422" alt="Screenshot 2023-02-23 at 17 30 39" src="https://user-images.githubusercontent.com/71371405/220984707-ff02f6e0-cbb2-49f1-9cc4-b06b5a8d1c1b.png">

<img width="555" alt="Screenshot 2023-02-24 at 12 57 53" src="https://user-images.githubusercontent.com/71371405/221184668-3e966db9-c950-447a-90a0-c979615227b0.png">

Congrats, you have a Pod! 

The Layers of abstraction 
1- Deployment manages a replicaset
2- ReplicaSet manages a Pod 
3- Pod is an abstraction of a container 
4- Everything lower is handled by kubernetes

<img width="1792" alt="Screenshot 2023-02-24 at 14 23 22" src="https://user-images.githubusercontent.com/71371405/221202232-6b69eb5b-7302-4549-8fbc-44732a95d2da.png">



If we need to see what is happening inside this pod type -> kubectle describe pod nginx
In a danger recovery scenario pods have Replicaset. Which manages the replicas of a pod. 
If you have chose to deploy you can see them. Type -> kubectl get replicaset.


So we have deployed a pod, but what if we wanted to edit it? Lets type...
-> kubectl get deployment
-> kubectl get pod replicaset
-> kubectl edit deployment nginx-depl
<img width="1792" alt="Screenshot 2023-02-24 at 13 12 10" src="https://user-images.githubusercontent.com/71371405/221192499-4c10fabb-b706-4991-8b50-d37cf6835318.png">
If you go to the image under container, you can change the version of deployment. This then create a new pod and will terminate the old one. 

Very important to note, if you need to debug a pod it is best to check the logs. So you will need the pod name. Type -> Kubectl get pod -> kubectl logs [pod name]. However we'll save this one for later as nginx hasn't log anthing yet.

And to delete the deployment, type -> kubectl delete deployment neginx-[name of your deployment]

<img width="1792" alt="Screenshot 2023-02-24 at 14 16 59" src="https://user-images.githubusercontent.com/71371405/221203130-c88d1ee3-566b-4937-8141-bdf6c2e5c82e.png">

We have done a lot so far, but how do put this in action? 

Project time! 












Next we will create a port and connect an image 



















