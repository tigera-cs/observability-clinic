

# Module 1 - Overview of the Calico Cloud Security Policy capabilities 

## 1. The lab architecture is explained in this topic


<p align="center">
  <img src="Images/1.m1lab1-1.jpg" alt="Network Diagram" align="center" width="500">
</p>

Hosts/nodes:

*  10.0.0.2/32: DNS Server for the subnet 10.0.1.0/24
*  10.0.1.1/32: Default gateway for the subnet 10.0.1.0/24
*  10.0.1.10/32: Bastion Host - Linux server that plays the roles of BGP ToR and Jump server
*  10.0.1.20/32: Kubernetes Master node (Control1) 
*  10.0.1.30/32: Kubernetes Worker node (Worker1)
*  10.0.1.31/32: Kubernetes Worker node (Worker2)

## 2. Install Calico as CNI

The nodes are in Not Ready status so it means there is no CNI configured.

```bash
kubectl get nodes
```
```bash
NAME                                         STATUS     ROLES                  AGE   VERSION
ip-10-0-1-20.ca-central-1.compute.internal   NotReady   control-plane,master   49m   v1.22.4
ip-10-0-1-30.ca-central-1.compute.internal   NotReady   worker                 49m   v1.22.4
ip-10-0-1-31.ca-central-1.compute.internal   NotReady   worker                 49m   v1.22.4
```

### a. Install the Calico Operator

```bash
kubectl apply -f https://projectcalico.docs.tigera.io/archive/v3.22/manifests/tigera-operator.yaml
```

### b. Verify if the Calico Operator has successfully deployed:

```bash
kubectl rollout status -n tigera-operator deployment tigera-operator
```
```bash
deployment "tigera-operator" successfully rolled out
```

### c. Download the custom resource manifest.

```bash
curl https://projectcalico.docs.tigera.io/archive/v3.22/manifests/custom-resources.yaml -O
```

### d. Change the POD CIDR from 192.168.0.0/16 to 10.48.0.0/16 and disable the encapsulation as per the commands below:

```bash
sed -i 's,192\.168\.0\.0\/16,10\.48\.0\.0\/16,g' custom-resources.yaml
sed -i 's,VXLANCrossSubnet,None,g' custom-resources.yaml
```

### e. Apply the custom-resource:

```bash
$ kubectl apply -f custom-resources.yaml
```

### f. Check if the node status is Ready:

```bash
kubectl get nodes
```
```bash
NAME                                         STATUS   ROLES                  AGE   VERSION
ip-10-0-1-20.ca-central-1.compute.internal   Ready    control-plane,master   61m   v1.22.4
ip-10-0-1-30.ca-central-1.compute.internal   Ready    worker                 60m   v1.22.4
ip-10-0-1-31.ca-central-1.compute.internal   Ready    worker                 60m   v1.22.4
```
### g. Check if the apiserver and calico are available:

```bash
$ kubectl get tigerastatus
```
```bash
NAME        AVAILABLE   PROGRESSING   DEGRADED   SINCE
apiserver   True        False         False      52s
calico      True        False         False      92s
```

## 3. Install Calico Cloud in the cluster

### a. In Calico Cloud UI, click in the Managed Cluster icon <img src="Images/icon-1.png" alt="Connect Cluster" width="30">, in the bottom “Connect Cluster”, insert the desired name for the cluster (put the cluster name), select the “Kubeadm” and click “Next”

