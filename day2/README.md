# docker-k8s-ocp-kyndryl

### revision 

<img src="rev.png">

### better than a physical server -= is testing or running app code in VM 

<img src="vm.png">

### VM limitations 

<img src="lim.png">

### Introduction to docker containers 

<img src="dc.png">

### verify docker connection 


```
[ashu@ip-172-31-91-107 ~]$ docker version 
Client:
 Version:           20.10.23
 API version:       1.41
 Go version:        go1.18.9
 Git commit:        7155243
 Built:             Tue Apr 11 22:56:36 2023
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.23
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.18.9
  Git commit:       6051f14
  Built:            Tue Apr 11 22:57:17 2023
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
```

### Docker env and container creation 

<img src="denv.png">

## try python code first 

### creating forlder to keep our code there

```
[ashu@ip-172-31-91-107 ~]$ whoami
ashu
[ashu@ip-172-31-91-107 ~]$ ls
[ashu@ip-172-31-91-107 ~]$ 
[ashu@ip-172-31-91-107 ~]$ mkdir  python-code java-code  webapp database
[ashu@ip-172-31-91-107 ~]$ ls
database  java-code  python-code  webapp
[ashu@ip-172-31-91-107 ~]$

```

### containerizing python code

### in python-code folder

### hello.py 

```
import time

while True:
    print("Hello all , welcome to python..!!")
    time.sleep(3)
    print("Welcome to Docker ..")
    time.sleep(2)
    print("Welcome to Containers ..!!")
    print("______________________")
    time.sleep(3)
```

### Dockerfile

```
FROM python
# FROM is gonna call python image env from Docker hub 
LABEL name="ashutoshh"
LABEL email="ashutoshh@linux.com"
# optional keyword in Dockerfile 
RUN mkdir /ashucode
# it is for running any command duration env creation in the docker image
COPY  hello.py /ashucode/hello.py 
# copy code to python env location
CMD ["python","/ashucode/hello.py"]
# cmd is only to run the code whenever you create container from this env image
 
```

### time to create env --(image) -- where you code and lang platform will be there

### check directory 

```
[ashu@ip-172-31-91-107 ~]$ ls
database  java-code  python-code  webapp
[ashu@ip-172-31-91-107 ~]$ ls  python-code/
Dockerfile  hello.py
```

### build env 

```
[ashu@ip-172-31-91-107 ~]$ ls
database  java-code  python-code  webapp
[ashu@ip-172-31-91-107 ~]$ ls  python-code/
Dockerfile  hello.py
[ashu@ip-172-31-91-107 ~]$ ls
database  java-code  python-code  webapp
[ashu@ip-172-31-91-107 ~]$ 
[ashu@ip-172-31-91-107 ~]$ docker  build   -t ashupython:v1   python-code/ 
Sending build context to Docker daemon  3.072kB
Step 1/6 : FROM python
latest: Pulling from library/python
785ef8b9b236: Pull complete 
5a6dad8f55ae: Pull complete 
bd36c7bfe5f4: Pull complete 
4d207285f6d2: Pull complete 
9402da1694b8: Pull complete 
9bdbf45d01af: Pull complete 
dd8b7ef87a9d: Pull complete 
4de52e7027c5: Pull complete 
Digest: sha256:9a1b705aecedc76e8bf87dfca091d7093df3f2dd4765af6c250134ce4298a584
Status: Downloaded newer image for python:latest
 ---> 608c79ebc6d5
Step 2/6 : LABEL name="ashutoshh"
 ---> Running in 4bdba788c3ed
Removing intermediate container 4bdba788c3ed
 ---> 89ed55beae3e
Step 3/6 : LABEL email="ashutoshh@linux.com"
 ---> Running in 0b4800507a34
Removing intermediate container 0b4800507a34
 ---> 77fc698f63a4
Step 4/6 : RUN mkdir /ashucode
 ---> Running in d8b9f350e465
Removing intermediate container d8b9f350e465
 ---> b5ab6d20923d
Step 5/6 : COPY  hello.py /ashucode/hello.py
 ---> 329742b3f0eb
Step 6/6 : CMD ["python","/ashucode/hello.py"]
 ---> Running in 9e002e913b66
Removing intermediate container 9e002e913b66
 ---> 942f59fc5524
Successfully built 942f59fc5524
Successfully tagged ashupython:v1
```

### verify images which is python env and the code 

```
[ashu@ip-172-31-91-107 ~]$ docker  images
REPOSITORY     TAG       IMAGE ID       CREATED          SIZE
yashnapython   v1        2116bf3896ce   29 seconds ago   1.01GB
ashupython     v1        942f59fc5524   30 seconds ago   1.01GB
ashpython      v1        6714cda7f47d   30 seconds ago   1.01GB
python         latest    608c79ebc6d5   6 weeks ago      1.01GB
```


