# docker-k8s-ocp-kyndryl

### docker and compose revision 

<img src="rev.png">

### k8s Internal understanding 

<img src="k8s.png">

### network flow of k8s app accessing 

<img src="appc.png">

## Introducing Openshift

<img src="oc.png">

### k8s vs OCP -- all you need to know as freshers

<img src="ocp.png">

### Removing k8s cred and putting it ocp cred

```
[ashu@ip-172-31-91-107 ~]$ cd  ~/.kube/
[ashu@ip-172-31-91-107 .kube]$ ls
cache  config
[ashu@ip-172-31-91-107 .kube]$ mv  config  k8s-cred.conf
[ashu@ip-172-31-91-107 .kube]$ ls
cache  k8s-cred.conf
[ashu@ip-172-31-91-107 .kube]$ pwd
/home/ashu/.kube
[ashu@ip-172-31-91-107 .kube]$ ls
cache  k8s-cred.conf
[ashu@ip-172-31-91-107 .kube]$ cp -v  /tmp/kubeconfig   .
‘/tmp/kubeconfig’ -> ‘./kubeconfig’
[ashu@ip-172-31-91-107 .kube]$ ls
cache  k8s-cred.conf  kubeconfig
[ashu@ip-172-31-91-107 .kube]$ mv  kubeconfig config 
[ashu@ip-172-31-91-107 .kube]$ ls
cache  config  k8s-cred.conf
```

### verify 

```
[ashu@ip-172-31-91-107 ~]$ oc version 
Client Version: 4.13.6
Kustomize Version: v4.5.7
Server Version: 4.13.6
Kubernetes Version: v1.26.6+73ac561
[ashu@ip-172-31-91-107 ~]$ 
```

### chekcing more things 

```
[ashu@ip-172-31-91-107 ~]$ oc  get  nodes
NAME                           STATUS   ROLES                  AGE   VERSION
ip-10-0-132-18.ec2.internal    Ready    control-plane,master   26h   v1.26.6+73ac561
ip-10-0-135-26.ec2.internal    Ready    worker                 26h   v1.26.6+73ac561
ip-10-0-135-57.ec2.internal    Ready    worker                 26h   v1.26.6+73ac561
ip-10-0-191-94.ec2.internal    Ready    control-plane,master   26h   v1.26.6+73ac561
ip-10-0-213-214.ec2.internal   Ready    control-plane,master   26h   v1.26.6+73ac561
[ashu@ip-172-31-91-107 ~]$ 
```

### Project as Smarter and having extra feature than namesapce

```
ashu@ip-172-31-91-107 ~]$ oc projects
You have access to the following projects and can switch between them with ' project <projectname>':

default
kube-node-lease
kube-public
kube-system
openshift
openshift-apiserver
openshift-apiserver-operator
openshift-authentication
openshift-authentication-operator
openshift-cloud-controller-manager
openshift-cloud-controller-manager-operator
openshift-cloud-credential-operator
openshift-cloud-network-config-controller
openshift-cluster-csi-drivers
```

## How to get webUI access 

### checking project which has webportal deployed

```
[ashu@ip-172-31-91-107 ~]$ oc projects   |  grep  console 
openshift-console
openshift-console-operator
openshift-console-user-settings
[ashu@ip-172-31-91-107 ~]$ 
[ashu@ip-172-31-91-107 ~]$ oc  get  pods
No resources found in default namespace.

[ashu@ip-172-31-91-107 ~]$ 
[ashu@ip-172-31-91-107 ~]$ oc  get  pods -n openshift-console
NAME                         READY   STATUS    RESTARTS        AGE
console-85d689885-9rm46      1/1     Running   1               26h
console-85d689885-h86mt      1/1     Running   1               26h
downloads-55ff47758f-ggl9p   1/1     Running   2 (3h22m ago)   26h
downloads-55ff47758f-rnb4h   1/1     Running   2 (3h22m ago)   26h

[ashu@ip-172-31-91-107 ~]$ oc  get  deploy  -n openshift-console
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
console     2/2     2            2           26h
downloads   2/2     2            2           26h

====>>
[ashu@ip-172-31-91-107 ~]$ oc  get  svc   -n openshift-console
NAME        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
console     ClusterIP   172.30.91.159   <none>        443/TCP   26h
downloads   ClusterIP   172.30.97.159   <none>        80/TCP

======>>
[ashu@ip-172-31-91-107 ~]$ oc  get  routes   -n openshift-console
NAME        HOST/PORT                                                   PATH   SERVICES    PORT    TERMINATION          WILDCARD
console     console-openshift-console.apps.dev-cluster.ashutoshh.in            console     https   reencrypt/Redirect   None
downloads   downloads-openshift-console.apps.dev-cluster.ashutoshh.in          downloads   http    edge/Redirect        None
```

### checking app status from OC -- got deployed by Webportal

```
[ashu@ip-172-31-91-107 ~]$ oc get  deploy
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
ashu-customer1        1/1     1            1           6m9s
ashwini-customer1     1/1     1            1           6m3s
nagashree-customer1   1/1     1            1           5m5s
nidhi-customer1       1/1     1            1           4m57s
rakshitha-customer1   1/1     1            1           5m53s
yashna-customer1      1/1     1            1           5m2s

[ashu@ip-172-31-91-107 ~]$ oc get  svc
NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP                            PORT(S)   AGE
ashu-customer1        ClusterIP      172.30.155.189   <none>                                 80/TCP    6m11s
ashwini-customer1     ClusterIP      172.30.9.144     <none>                                 80/TCP    6m6s
kubernetes            ClusterIP      172.30.0.1       <none>                                 443/TCP   11m
nagashree-customer1   ClusterIP      172.30.242.97    <none>                                 80/TCP    5m8s
nidhi-customer1       ClusterIP      172.30.169.164   <none>                                 80/TCP    5m
openshift             ExternalName   <none>           kubernetes.default.svc.cluster.local   <none>    10m
rakshitha-customer1   ClusterIP      172.30.137.252   <none>                                 80/TCP    5m56s
yashna-customer1      ClusterIP      172.30.84.56     <none>                                 80/TCP    5m5s

[ashu@ip-172-31-91-107 ~]$ oc get  routes
NAME                  HOST/PORT                                                   PATH   SERVICES              PORT     TERMINATION     WILDCARD
ashu-customer1        ashu-customer1-default.apps.dev-cluster.ashutoshh.in               ashu-customer1        80-tcp   edge/Redirect   None
ashwini-customer1     ashwini-customer1-default.apps.dev-cluster.ashutoshh.in            ashwini-customer1     80-tcp   edge/Redirect   None
nagashree-customer1   nagashree-customer1-default.apps.dev-cluster.ashutoshh.in          nagashree-customer1   80-tcp   edge/Redirect   None
nidhi-customer1       nidhi-customer1-default.apps.dev-cluster.ashutoshh.in              nidhi-customer1       80-tcp   edge/Redirect   None
rakshitha-customer1   rakshitha-customer1-default.apps.dev-cluster.ashutoshh.in          rakshitha-customer1   80-tcp   edge/Redirect   None
yashna-customer1      yashna-customer1-default.apps.dev-cluster.ashutoshh.in             yashna-customer1      80-tcp   edge/Redirect   None
[ashu@ip-172-31-91-107 ~]$ 
```

### oc commands

```
708  oc run  ashu-pypod --image=docker.io/dockerashu/ashunew:ocimgv1 --dry-run=client -o yaml 
  709  kubectl explain pod.spec.containers | grep -i tty
  710  kubectl explain pod.spec.containers.tty
  711  oc run  ashu-pypod --image=docker.io/dockerashu/ashunew:ocimgv1 --dry-run=client -o yaml 
  712  oc get  pods
  713  oc project ashwini-new 
  714  oc get  pods
  715  oc logs  -f ashu-pypod 
  716  hsitor
  717  history 
[ashu@ip-172-31-91-107 ~]$ oc get pods
NAME                      READY   STATUS    RESTARTS   AGE
ashu-pypod                1/1     Running   0          88s
ashwini-59b85b6b5-t4zwv   1/1     Running   0          13m
[ashu@ip-172-31-91-107 ~]$ oc delete pod  ashu-pypod
pod "ashu-pypod" deleted
```


