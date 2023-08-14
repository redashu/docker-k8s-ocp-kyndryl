# docker-k8s-ocp-kyndryl

### OC vs k8s 

<img src="oc1.png">

### checking current project and cleaning up the data

```
[ashu@ip-172-31-91-107 ~]$ oc project
Using project "ashu-day10" on server "https://api.dev-cluster.ashutoshh.in:6443".
[ashu@ip-172-31-91-107 ~]$ oc get  all
NAME                           READY   STATUS    RESTARTS   AGE
pod/ashu-dep-dc4c58857-b57cb   1/1     Running   1          2d22h

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ashu-dep   1/1     1            1           2d22h

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/ashu-dep-dc4c58857   1         1         1       2d22h
[ashu@ip-172-31-91-107 ~]$ oc delete all --all
pod "ashu-dep-dc4c58857-b57cb" deleted
deployment.apps "ashu-dep" deleted
replicaset.apps "ashu-dep-dc4c58857" deleted

```
