# Kubernate Cluster Build

Prepare 4 Server for the Kubernates cluster  
|Host Name|IP Address|MASK|Gateway|DNS|CPU|Memory|OS|
|---------|----------|----|-------|---|---|------|--|
|master-node|192.168.0.100|255.255.255.0|192.168.0.1|8.8.8.8|2CPU|4GB|Cent OS 7|
|worker-node-1|192.168.0.101|255.255.255.0|192.168.0.1|8.8.8.8|2CPU|4GB|Cent OS 7|
|worker-node-2|192.168.0.102|255.255.255.0|192.168.0.1|8.8.8.8|2CPU|4GB|Cent OS 7|
|worker-node-3|192.168.0.103|255.255.255.0|192.168.0.1|8.8.8.8|2CPU|4GB|Cent OS 7|

### Configure All Nodes
  - Set host name of each node
```sh
    run command on Master node = hostnamectl set-hostname master-node
    run command on worker-node-1= hostnamectl set-hostname worker-node-1
    run command on worker-node-2 = hostnamectl set-hostname worker-node-2
    run command on worker-node-3 = hostnamectl set-hostname worker-node-3
```
  - Update host file 
```sh
    cat <<EOF>> /etc/hosts
    192.168.0.100 master-node
    192.168.0.101 node-1 worker-node-1
    192.168.0.102 node-2 worker-node-2
    192.168.0.103 node-3 worker-node-3
    EOF
```

  - Disable SElinux and update firewall rules
```sh
     run command on all 4 Nodes
       setenforce 0
       sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
       systemctl stop firewalld
       systemctl disable firewalld
```

  - Update DNS File.
```sh
cat >/etc/resolve.conf
nameserver 8.8.8.8
```

  - Disable swap
```sh
vi /etc/fstab
open /etc/fstab file, search for the swap line and comment the entire line by adding a # (hashtag) sign in front of the line
```