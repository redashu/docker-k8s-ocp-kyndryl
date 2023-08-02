# docker-k8s-ocp-kyndryl

### Revision 

<img src="rev.png">

### container build and run process

<img src="rev1.png">

### connecting to Lab 

```
[ashu@ip-172-31-91-107 ~]$ whoami
ashu
[ashu@ip-172-31-91-107 ~]$ docker images  | grep ashu
ashuhttpd         v1        01a983cf482a   20 hours ago    483MB
ashujava          v1        cb69966deff9   21 hours ago    470MB
ashupython        v38       48d02515ece5   22 hours ago    997MB
ashupython        v1        942f59fc5524   23 hours ago    1.01GB
[ashu@ip-172-31-91-107 ~]$ 

```

### containerizing sample frontend based webapp 

```
[ashu@ip-172-31-91-107 ~]$ ls
database  java-code  python-code  webapp
[ashu@ip-172-31-91-107 ~]$ git clone  https://github.com/codingstella/vCard-personal-portfolio.git
Cloning into 'vCard-personal-portfolio'...
remote: Enumerating objects: 69, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 69 (delta 3), reused 8 (delta 0), pack-reused 53
Receiving objects: 100% (69/69), 1.14 MiB | 26.60 MiB/s, done.
Resolving deltas: 100% (4/4), done.
[ashu@ip-172-31-91-107 ~]$ ls
database  java-code  python-code  vCard-personal-portfolio  webapp
[ashu@ip-172-31-91-107 ~]$ 


```

### adding Dockerifle and .dockerignore to the source code

### Dockerfile
```
FROM oraclelinux:8.4 
LABEL name="ashutoshh"
RUN yum install httpd -y 
COPY .  /var/www/html/
# . means all the files from current location to /var/www/html
CMD ["httpd","-DFOREGROUND"]
```

### .dockerignore

```
Dockerfile
.dockerignore
.git
index.txt
README.md
```

### building image of webbapp

```
ashu@ip-172-31-91-107 ~]$ ls
ashu-website  database  java-code  python-code  webapp
[ashu@ip-172-31-91-107 ~]$ cd  ashu-website/

[ashu@ip-172-31-91-107 ashu-website]$ ls
assets  Dockerfile  index.html  index.txt  README.md  website-demo-image
[ashu@ip-172-31-91-107 ashu-website]$

[ashu@ip-172-31-91-107 ashu-website]$ docker build -t  ashu-webapp:v1  .  
Sending build context to Docker daemon  1.305MB
Step 1/5 : FROM oraclelinux:8.4
 ---> 97e22ab49eea
Step 2/5 : LABEL name="ashutoshh"
 ---> Using cache
 ---> 598954f9b1d6
Step 3/5 : RUN yum install httpd -y
 ---> Using cache
 ---> 0435b09f5083
Step 4/5 : COPY .  /var/www/html/
 ---> 293fd1e6ab0a
Step 5/5 : CMD ["httpd","-DFOREGROUND"]
 ---> Running in 22363b994bac
Removing intermediate container 22363b994bac
 ---> 4d7a9b6ce0fe
Successfully built 4d7a9b6ce0fe
Successfully tagged ashu-webapp:v1
```

