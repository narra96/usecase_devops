# Installation of k8
## Pre requirements 

>sudo su 

>apt-get update

>swapoff -a 

>nano /etc/fstab #comment swap line 

>nano /etc/hostname

>rename hostname as kmaster

#staticip address

>ifconfig

#set static ip address 

>nano /etc/network/interfaces

#set rules for static ip  make sure it is defined correctly as below 

auto enp0s8

iface enp0s8 inet static

address <IP-Address-Of-VM>

#Now after adding this we get back to the host to update the ip address here 

>nano /etc/hostname
#here add ip address and host name as kmaster 

#now we need to restart the machine and check the host name changed to kmaster or not later check the ip address is same or not 

# Install OpenSSH
sudo apt-get install openssh-server 

# Install docker 
https://docs.docker.com/engine/install/ubuntu/

# Installation of k8#

1. Enter the following to add a signing key:

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add

If you get an error that curl is not installed, install it with:

sudo apt-get install curl

2. Then repeat the previous command to install the signing keys. Repeat for each server node.

Step 4: Add Software Repositories

Kubernetes is not included in the default repositories. To add them, enter the following:

sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

Repeat on each server node.

Step 5: Kubernetes Installation Tools

Kubeadm (Kubernetes Admin) is a tool that helps initialize a cluster. It fast-tracks setup by using community-sourced best practices. Kubelet is the work package, which runs on every node and starts containers. The tool gives you command-line access to clusters.

## Install Kubernetes tools with the command:
step 1 :

sudo apt-get install kubeadm kubelet kubectl

sudo apt-mark hold kubeadm kubelet kubectl

Allow the process to complete.

2. Verify the installation with:

kubeadm version

3. Repeat for each server node.

***Note: Make sure you install the same version of each package on each machine. Different versions can create instability. Also, this process prevents apt from automatically updating Kubernetes. For update instructions, please see the developers’ instructions.***

## Kubernetes Deployment

Step 6: Begin Kubernetes Deployment

Start by disabling the swap memory on each server:

sudo swapoff –a

Step 7: Assign Unique Hostname for Each Server Node 
Decide which server to set as the master node. Then enter the command:

sudo hostnamectl set-hostname master-node

Next, set a worker node hostname by entering the following on the worker server:

sudo hostnamectl set-hostname worker01

If you have additional worker nodes, use this process to set a unique hostname on each.

Step 8: Initialize Kubernetes on Master Node

Switch to the master server node, and enter the following:

sudo kubeadm init --pod-network-cidr=10.244.0.0/16

Once this command finishes, it will display a kubeadm join message at the end. Make a note of the whole entry. This will be used to join the worker nodes to the cluster.

Next, enter the following to create a directory for the cluster:

kubernetes-master:~$ mkdir -p $HOME/.kube

kubernetes-master:~$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

kubernetes-master:~$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

Step 9: Deploy Pod Network to Cluster

A Pod Network is a way to allow communication between different nodes in the cluster. This tutorial uses the flannel virtual network.

Enter the following:

sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

Allow the process to complete.

Verify that everything is running and communicating:

kubectl get pods --all-namespaces

Step 10: Join Worker Node to Cluster

As indicated in Step 7, you can enter the kubeadm join command on each worker node to connect it to the cluster.

Switch to the worker01 system and enter the command you noted from Step 7:

kubeadm join --discovery-token abcdef.1234567890abcdef --discovery-token-ca-cert-hash sha256:1234..cdef 1.2.3.4:6443

Replace the alphanumeric codes with those from your master server. Repeat for each worker node on the cluster. Wait a few minutes; then you can check the status of the nodes.

Switch to the master server, and enter:

kubectl get nodes

The system should display the worker nodes that you joined to the cluster.



