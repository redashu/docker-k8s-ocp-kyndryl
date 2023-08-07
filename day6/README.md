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

## A manifest file -- for creating Pod 

### Sending Create Request to APiserver

```
[ashu@ip-172-31-91-107 ~]$ ls
admin.conf    ashu-customer  ashu-website  final-project-ashu  k8s-docs   python-code
ashu-compose  ashu-task4     database      java-code           k8s-files  webapp
[ashu@ip-172-31-91-107 ~]$ cd k8s-files/
[ashu@ip-172-31-91-107 k8s-files]$ ls
ashupod1.yaml
[ashu@ip-172-31-91-107 k8s-files]$ kubectl  create -f  ashupod1.yaml 
pod/ashupod created
[ashu@ip-172-31-91-107 k8s-files]$ kubectl  get  pods
NAME         READY   STATUS    RESTARTS   AGE
ashupod      1/1     Running   0          7s
ashwinipod   1/1     Running   0          8s
[ashu@ip-172-31-91-107 k8s-files]$ 
```

### Understanding Etcd component of control plane machine 

<img src="etcd.png">

### creating a pod manifest with multi container concept 

```
apiVersion: v1 
kind: Pod 
metadata: # info about kind  
  name: ashupod  # name of my first pod 
spec: # all the details about your app like volume,security,containers
  containers:
  - name: ashuc2
    image: alpine 
    command: ['sh','-c','sleep 10000']
  - name: ashuc1 
    image: docker.io/dockerashu/ashu-customer1:releasev1
    ports: # optional part 
    - containerPort: 80 # app server port of docker image
```

### lets apply changes

```
[ashu@ip-172-31-91-107 k8s-files]$ kubectl replace -f  ashupod1.yaml  --force 
pod "ashupod" deleted
pod/ashupod replaced
[ashu@ip-172-31-91-107 k8s-files]$ kubectl  get  pods
NAME           READY   STATUS        RESTARTS   AGE
ashupod        2/2     Running       0          9s
```

