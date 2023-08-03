# docker-k8s-ocp-kyndryl

### Revision 

<img src="rev.png">

### custom story with docker 

<img src="cust1.png">

### taking 3 sample applications from github 

```
 222  mkdir  ashu-customer
  224  cd  ashu-customer/
  226  git clone https://github.com/schoolofdevops/html-sample-app
  227  git clone https://github.com/microsoft/project-html-website
  228  git clone https://github.com/Yash-srivastav16/Tour-Project
```

### adding Dockerfile into it 

```
FROM oraclelinux:8.4
LABEL email="ashutoshh@linux.com"
ENV web=hello
# to create ENV variable and some value 
RUN yum install httpd -y 
RUN mkdir /code  /code/webapp1 /code/webapp2 /code/webapp3 
COPY html-sample-app /code/webapp1/
COPY project-html-website /code/webapp2/
ADD Tour-Project /code/webapp3/
# COPY and ADD both are almost same 
# add can take input from URL as well
COPY deploy.sh /code/
WORKDIR /code
# changing directory 
RUN chmod +x deploy.sh 
CMD ["./deploy.sh"]
```

### shell script 

```
#!/bin/bash

if  [ "$web" == "myapp1" ]
then
    cp -rf /code/webapp1/ /var/www/html/
    httpd -DFOREGROUND 
elif [ "$web" == "myapp2" ]
then
    cp -rf /code/webapp2/ /var/www/html/
    httpd -DFOREGROUND 
elif [ "$web" == "myapp3" ]
then
    cp -rf /code/webapp3/ /var/www/html/
    httpd -DFOREGROUND
else 
    echo "Please check your variable or Value" >/var/www/html/index.html
    httpd -DFOREGROUND
fi 
```


