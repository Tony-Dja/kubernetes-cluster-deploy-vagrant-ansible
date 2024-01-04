# kubernetes-cluster-multinode-vagrant-and-ansible

Prérequis :

1 Install VirtualBox

- import gpg key of Virtualbox repository

$ sudo apt install curl wget gnupg2 lsb-release -y
$ curl -fsSL https://www.virtualbox.org/download/oracle_vbox_2016.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/vbox.gpg
$ curl -fsSL https://www.virtualbox.org/download/oracle_vbox.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/oracle_vbox.gpg

- add virtualbox repository

$ echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list

- install VirtualBox

$ sudo apt update
$ sudo apt install linux-headers-$(uname -r) dkms -y
$ sudo apt install virtualbox-7.0 -y


2 Install Vagrant

3 Install Ansible
