<div align="center">

<img src="https://github.com/Tony-Dja/kubernetes-cluster-deploy-vagrant-ansible/blob/35e60ca64e9bc44f5352709d64ff3f654cdc9ae1/screenshots/kubernetes-cluster.gif">

# Cluster Kubernetes

Déploiement automatisé d'un `cluster Kubernetes multi-noeuds` au sein d'un environnement virtuel `Vagrant`.

</div>

## Environnement local

- [ ] OS - Debian 12 Bookworm
- [ ] Oracle Virtualbox 7.0
- [ ] Vagrant 2.4.0
- [ ] Ansible 2.15.8

## Pré-requis

Installer les outils suivants :

- `VirtualBox` => Déploiement des VMs
- `Vagrant` => Création d'un Environnement virtuel
- `Ansible` => Provisionnement des ressources du Cluster


#### Installation de VirtualBox

- [ ] import gpg key of Virtualbox repository

```sh 
sudo apt install curl wget gnupg2 lsb-release -y $ curl -fsSL https://www.virtualbox.org/download/oracle_vbox_2016.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/vbox.gpg $ curl -fsSL https://www.virtualbox.org/download/oracle_vbox.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/oracle_vbox.gpg
```

- [ ] add virtualbox repository

```sh
echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list
```

- [ ] install VirtualBox

```sh
sudo apt update $ sudo apt install linux-headers-$(uname -r) dkms -y $ sudo apt install virtualbox-7.0 -y
```


#### Installation de Vagrant


### Deployer le cluster et lancer les noeuds

```sh
vagrant up
```

### Configurer le cluster

La configuration du cluster est gérée dans les variables présentes dans le fichier `Vagrantfile`

Il est possible de modifier les paramètres suivants :

- [ ] Image du système d'exploitation
- [ ] Mémoire RAM
- [ ] Nombre de CPU
- [ ] Nom du Master Node
- [ ] Nombre de Worker Node
- [ ] Adresses IP du réseaux de Noeuds
- [ ] Adresses IP du réseaux de Pods


### Se connecter au cluster via ssh

A partir du dossier où se trouve le `Vagrantfile` :

```sh
vagrant ssh master
```

A partir d'un autre emplacement:

```sh
ssh -r vagrant@192.168.56.10
password = vagrant
```

### Configurer kubectl 

Vous pouvez configurer `kubectl` sur votre machine locale afin de communiquer avec l'`API Kubernetes`.

Copier sur votre machine locale le dossier `.kube` contenant le fichier de configuration du cluster :

```sh
scp -r vagrant@192.168.56.10:/home/vagrant/.kube $HOME/
password = vagrant
```

### Tester la configuration du cluster

Vérifier que les Noeuds sont bien présents avec un status `Ready` :

```sh
kubectl get nodes -ALL -o wide
```

`Résultat :`

```
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP     OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
master     Ready    control-plane   99m   v1.28.2   192.168.56.10   Ubuntu 18.04.6 LTS   4.15.0-206-generic   containerd://1.6.21   
worker-1   Ready    <none>          96m   v1.28.2   192.168.56.11   Ubuntu 18.04.6 LTS   4.15.0-206-generic   containerd://1.6.21
```


Vérifier que les Pods sont bien présents avec un status `Running` :

```sh
kubectl get pods -ALL -o wide
```

`Résultat :`

```
NAMESPACE      NAME                             READY   STATUS    RESTARTS   AGE   IP              NODE       
kube-flannel   kube-flannel-ds-gk8fn            1/1     Running   0          98m   192.168.56.10   master                
kube-flannel   kube-flannel-ds-zwxs2            1/1     Running   0          96m   192.168.56.11   worker-1              
kube-system    coredns-5dd5756b68-stn9l         1/1     Running   0          98m   10.244.0.3      master                
kube-system    coredns-5dd5756b68-wbk4t         1/1     Running   0          98m   10.244.0.2      master                 
kube-system    etcd-master                      1/1     Running   0          99m   192.168.56.10   master                 
kube-system    kube-apiserver-master            1/1     Running   0          99m   192.168.56.10   master                
kube-system    kube-controller-manager-master   1/1     Running   0          99m   192.168.56.10   master                
kube-system    kube-proxy-nlhbd                 1/1     Running   0          98m   192.168.56.10   master                 
kube-system    kube-proxy-tqzq4                 1/1     Running   0          96m   192.168.56.11   worker-1              
kube-system    kube-scheduler-master            1/1     Running   0          99m   192.168.56.10   master      
```
