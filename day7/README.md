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

### creating nodeport service by exposing pod 

```
[ashu@ip-172-31-91-107 day7]$ kubectl get pods
NAME          READY   STATUS    RESTARTS   AGE
ashu-webpod   1/1     Running   0          3m57s
[ashu@ip-172-31-91-107 day7]$ kubectl  expose pod  ashu-webpod  --type NodePort --port 80 --name ashulb1 --dry-run=client -o yaml  >nodeport.yaml 
[ashu@ip-172-31-91-107 day7]$ ls
nodeport.yaml  pod1.yaml
[ashu@ip-172-31-91-107 day7]$ kubectl  create -f nodeport.yaml 
service/ashulb1 created
[ashu@ip-172-31-91-107 day7]$ kubectl get svc
NAME      TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
ashulb1   NodePort   10.103.12.59   <none>        80:31333/TCP   3s
[ashu@ip-172-31-91-107 day7]$ kubectl get service
NAME      TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
ashulb1   NodePort   10.103.12.59   <none>        80:31333/TCP   7s
[ashu@ip-172-31-91-107 day7]$ 

```

## Deployment controller introduction 

<img src="depc.png">

### deleting pod,svc 

```
[ashu@ip-172-31-91-107 day7]$ kubectl get po,svc
NAME              READY   STATUS    RESTARTS   AGE
pod/ashu-webpod   1/1     Running   0          22m

NAME              TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
service/ashulb1   NodePort   10.103.12.59   <none>        80:31333/TCP   17m
[ashu@ip-172-31-91-107 day7]$ 
[ashu@ip-172-31-91-107 day7]$ kubectl  delete pod,svc --all
pod "ashu-webpod" deleted
service "ashulb1" deleted

```

### creating deployment controller based pods

```
 kubectl  create  deployment  ashu-app-deploy  --image=docker.io/dockerashu/ashu-customer1:releasev1 --port 80 --dry-run=client -o yaml >deployment1.yaml
```

### updating manifest 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-app-deploy
  name: ashu-app-deploy # name of deployment 
spec:
  replicas: 1 # number of pods we want 
  selector:
    matchLabels:
      app: ashu-app-deploy
  strategy: {}
  template: # deployment will be using pod template to create multiple pods
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-app-deploy
    spec:
      containers:
      - image: docker.io/dockerashu/ashu-customer1:releasev1
        name: ashu-customer1
        ports:
        - containerPort: 80
        resources: {}
        env:
        - name: web
          value: myapp3 
status: {}

```

### creating it 

```
[ashu@ip-172-31-91-107 day7]$ kubectl  create -f deployment1.yaml 
deployment.apps/ashu-app-deploy created
[ashu@ip-172-31-91-107 day7]$ 
[ashu@ip-172-31-91-107 day7]$ kubectl get  deployment 
NAME              READY   UP-TO-DATE   AVAILABLE   AGE
ashu-app-deploy   1/1     1            1           5s
[ashu@ip-172-31-91-107 day7]$ kubectl  get  pods
NAME                               READY   STATUS    RESTARTS   AGE
ashu-app-deploy-64bcd577fb-sw4zf   1/1     Running   0          9s
[ashu@ip-172-31-91-107 day7]$ 

```




