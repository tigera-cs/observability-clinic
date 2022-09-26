

# Module 1 - Overview of the Calico Cloud Security Policy capabilities 

## 1. The lab architecture is explained in this topic


<p align="center">
  <img src="Images/1.m1lab1-1.jpg" alt="Network Diagram" align="center" width="400">
</p>

Hosts/nodes:

• 10.0.0.2/32: DNS Server for the subnet 10.0.1.0/24
• 10.0.1.1/32: Default gateway for the subnet 10.0.1.0/24
• 10.0.1.10/32: Bastion Host - Linux server that plays the roles of BGP ToR and Jump server
• 10.0.1.20/32: Kubernetes Master node (Control1) 
• 10.0.1.30/32: Kubernetes Worker node (Worker1)
• 10.0.1.31/32: Kubernetes Worker node (Worker2)

## 2. Install Calico as CNI

The nodes are in Not Ready status so it means there is no CNI configured.

```bash
kubectl get nodes
NAME                                         STATUS     ROLES                  AGE   VERSION
ip-10-0-1-20.ca-central-1.compute.internal   NotReady   control-plane,master   49m   v1.22.4
ip-10-0-1-30.ca-central-1.compute.internal   NotReady   worker                 49m   v1.22.4
ip-10-0-1-31.ca-central-1.compute.internal   NotReady   worker                 49m   v1.22.4
```