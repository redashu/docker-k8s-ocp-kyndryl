# docker-k8s-ocp-kyndryl

### Revision of Docker 

<img src="rev1.png">

### Revision of k8s 

<img src="rev2.png">

### verify lab connection 

```
[ashu@ip-172-31-91-107 ~]$ kubectl  get  nodes
NAME     STATUS   ROLES           AGE   VERSION
master   Ready    control-plane   3d    v1.27.4
node1    Ready    <none>          3d    v1.27.4
node2    Ready    <none>          3d    v1.27.4
[ashu@ip-172-31-91-107 ~]$ kubectl  cluster-info 
Kubernetes control plane is running at https://172.31.86.69:6443
CoreDNS is running at https://172.31.86.69:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
[ashu@ip-172-31-91-107 ~]$ 
```


