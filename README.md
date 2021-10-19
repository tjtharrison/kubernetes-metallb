# kubernetes-metallb

Configuration files for installing MetalLB Load Balancer. MetalLB is used to setup Load Balancer services on a Baremetal Kubernetes installation.

# Installation

Before deploying MetalLB update the configMap yaml in this repository with an IP address range that can be allocated to MetalLB. Ensure this is outside of the DHCP scope for Your network.

IP addresses from this range will be allocated to Your Services as they are deployed.

Once You have updated the config map, use kubectl to apply the yaml files.

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/main/manifests/namespace.yaml
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
kubectl apply -f metallb-configmap.yml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/main/manifests/metallb.yaml
```

Wait until all of the pods have been deployed and are running:

```
watch -n 2 kubectl get pods -n metallb-system
```

Finally apply the configMap