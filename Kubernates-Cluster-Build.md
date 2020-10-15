# Kubernate Cluster Build

Prepare 4 Server for the Kubernates cluster  
|Host Name|IP Address|MASK|Gateway|DNS|CPU|Memory|OS|
|---------|----------|----|-------|---|---|------|--|
|master-node|192.168.0.100|255.255.255.0|192.168.0.1|8.8.8.8|2CPU|4GB|Cent OS 7|
|worker-node-1|192.168.0.101|255.255.255.0|192.168.0.1|8.8.8.8|2CPU|4GB|Cent OS 7|
|worker-node-2|192.168.0.102|255.255.255.0|192.168.0.1|8.8.8.8|2CPU|4GB|Cent OS 7|
|worker-node-3|192.168.0.103|255.255.255.0|192.168.0.1|8.8.8.8|2CPU|4GB|Cent OS 7|

### Configure All Nodes
* Set host name of each node
```sh
	run command on Master node = hostnamectl set-hostname master-node
    	run command on worker-node-1= hostnamectl set-hostname worker-node-1
    	run command on worker-node-2 = hostnamectl set-hostname worker-node-2
    	run command on worker-node-3 = hostnamectl set-hostname worker-node-3
```
* Update host file(run command on all 4 Nodes)
```sh
	cat <<EOF>> /etc/hosts
	192.168.0.100 master-node
	192.168.0.101 node-1 worker-node-1
    	192.168.0.102 node-2 worker-node-2
    	192.168.0.103 node-3 worker-node-3
	EOF
```
* Disable SElinux and update firewall rules(run command on all 4 Nodes)
```sh
	setenforce 0
	sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
	systemctl stop firewalld
	systemctl disable firewalld
```
* Update DNS File.
```sh
	cat >/etc/resolve.conf
	nameserver 8.8.8.8
```
* Disable swap
```sh
		open /etc/fstab file, search for the swap line and comment the entire line by adding a # (hashtag) sign in front of the line
		vi /etc/fstab
```
* Reboot
```sh
	reboot
```
* Install & Configure Docker (All Nodes)
```sh
	Remove podman if installed
        yum remove pod* -y
    
	Enable Docker CE Repository
        dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
	
	List docker Repository
        dnf list docker-ce
    	
	Install docker
        dnf install docker-ce --nobest -y
    	
	Configure Docker Service
        systemctl enable docker
        systemctl start docker
```
* Install Kubernetes

```sh
	Update repo file

	cat <<EOF > /etc/yum.repos.d/kubernetes.repo
	[kubernetes]
	name=Kubernetes
	baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
	enabled=1
	gpgcheck=1
	repo_gpgcheck=1
	gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
	EOF

	Install Kubernate package 
	yum install kubeadm -y

	Configure Kubernates Service
	systemctl enable kubelet
	systemctl start kubelet
```
* Configure Master Node(Kube Cluster)
```sh
	Initate Cluster.
	kubeadm init --pod-network-cidr=10.244.0.0/16
        
	Copy output of above command and run on master node(like Below)
     		mkdir -p $HOME/.kube
		sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
		sudo chown $(id -u):$(id -g) $HOME/.kube/config
	
	Copy output of above command and run on worker nodes to join cluster((like Below)
		kubeadm join 192.168.0.101:6443 --token ipcm0q.gvlksw3ytdf14oel \ --discovery-token-ca-cert-hash sha256:7af12851ed2021a9432c722470c399078b41503c06ea37af9198d55b2d1b2878

	Configure Pod Networking(Master Node).
		kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
* Check Node Status
```
	Kubectl get nodes
```