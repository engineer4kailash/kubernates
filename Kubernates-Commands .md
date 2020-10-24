# Kubernate Important Commands

## Commands with basic output
|Command |Description|
|--------|-----------|
|kubectl get services|				|List all services in the namespace|
|kubectl get pods --all-namespaces		|List all pods in all namespaces|
|kubectl get pods -o wide			|List all pods in the current namespace, with more details|
|kubectl get deployment my-dep			|List a particular deployment|
|kubectl get pods				|List all pods in the namespace|
|kubectl get pod my-pod -o yaml 		|Get a pod's YAML|
|kubectl get nodes -o json			|Get a pod's json|
|kubectl get pods --all-namespaces		|All pods in All NameSpaces|
|kubectl get pv					|List PV|
|kubeadm token create --print-join-command	|Cluster Token for Worker nodes to join Cluster|
|kubectl cordon $NODENAME  			|To mark a Node unschedulable|

## Compares the current state of the cluster against the state that the cluster would be in if the manifest was applied.
kubectl diff -f ./my-manifest.yaml