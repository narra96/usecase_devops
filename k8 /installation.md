# Installation of k8

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

