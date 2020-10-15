# Kubernate Cluster Build

Prepare 3 Server for the Kubernates cluster  
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