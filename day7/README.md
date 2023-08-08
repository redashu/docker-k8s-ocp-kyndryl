# docker-k8s-ocp-kyndryl
### k8s Network flow understanding 

<img src="netflow.png">

## namespace in k8s

<img src="k8sns.png">

### listing namespaces of k8s 

```
[ashu@ip-172-31-91-107 ~]$ kubectl   get   namespaces 
NAME                   STATUS   AGE
default                Active   4d
kube-node-lease        Active   4d
kube-public            Active   4d
kube-system            Active   4d
kubernetes-dashboard   Active   4d
```

### creating namespace

```
[ashu@ip-172-31-91-107 ~]$ kubectl create  namespace  ashu-project
namespace/ashu-project created
[ashu@ip-172-31-91-107 ~]$ kubectl   get   namespaces 
NAME                   STATUS   AGE
ashu-project           Active   2s
default                Active   4d
```

### setting namespace as default 

```
[ashu@ip-172-31-91-107 ~]$ kubectl  config set-context --current --namespace=ashu-project
Context "kubernetes-admin@kubernetes" modified.
[ashu@ip-172-31-91-107 ~]$ kubectl get pods
No resources found in ashu-project namespace.
[ashu@ip-172-31-91-107 ~]$ 
```

### creating pod 

```
[ashu@ip-172-31-91-107 ~]$ mkdir  day7
[ashu@ip-172-31-91-107 ~]$ cd day7/
[ashu@ip-172-31-91-107 day7]$ kubectl run ashu-webpod --image=docker.io/dockerashu/ashu-customer1:releasev1 --port 80 --dry-run=client -o yaml >pod1.yaml 
[ashu@ip-172-31-91-107 day7]$ 


```

### updating ENV in pod manifest

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashu-webpod
  name: ashu-webpod
spec:
  containers:
  - image: docker.io/dockerashu/ashu-customer1:releasev1
    name: ashu-webpod
    ports:
    - containerPort: 80
    resources: {}
    env:
    - name: web
      value: myapp2 
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

### creating it 

```
[ashu@ip-172-31-91-107 day7]$ ls
pod1.yaml
[ashu@ip-172-31-91-107 day7]$ kubectl  create -f pod1.yaml 
pod/ashu-webpod created
[ashu@ip-172-31-91-107 day7]$ kubectl  get  pods
NAME          READY   STATUS    RESTARTS   AGE
ashu-webpod   1/1     Running   0          4s
[ashu@ip-172-31-91-107 day7]$ 


```



