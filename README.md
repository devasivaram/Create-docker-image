# Performing flask application for creating docker image

## Description

We are creating an custom docker image with help of existing docker image and we are creating an container using the custom/created docker image. Let's move on to the process.

## Installation

If you need to download docker to the EC2 instance, then follow this:

~~~sh
sudo yum install docker -y
sudo systemctl restart docker.service
sudo systemctl enable docker.service
sudo systemctl status docker.service
sudo usermod -a -G docker ec2-user
~~~

Please exit from the instance and login back to reflect the above changes

## Procedure

## Create directory and files required

~~~sh
mkdir flask-app
cd flask-app/
touch app.py requirements.txt Dockerfile
~~~

## Add code to app.py

~~~sh
vim app.py
~~~
> Am adding simple flask application:

~~~
from flask import Flask
app = Flask(__name__)

@app.route("/")
def index():
  return "<h1><center>Devanand - version 1</center></h1>"

if __name__ == "__main__":
  app.run(host='0.0.0.0',port=5000)
~~~

## Add to requirements.txt

~~~sh
vim requirements.txt
~~~
> Add:
~~~
flask
~~~

## Create Dockerfile 

~~~sh
vim Dockerfile
~~~

> Add these for building image

~~~

FROM alpine:3.8

RUN  mkdir  /var/flaskapp

WORKDIR  /var/flaskapp

COPY  .   .

RUN  apk update && apk add --no-cache python3

RUN  pip3 install -r requirements.txt

EXPOSE 5000

CMD ["python3" , "app.py"]
~~~

**Lets start to build the docker image**

### To build docker image 

~~~sh
docker image build -t dockertest:1 .
~~~
>Result

![image](https://user-images.githubusercontent.com/100773863/162553606-604eabbf-adb8-4274-b058-107a25263cb7.png)


## Pushing custom image to Docker

Now, let's add this custom docker image to our docker hub. First we need to sign up in [docker hub](https://hub.docker.com/) and then follow this:

~~~sh
docker login
~~~
>pass your credentials and you will get login succeeded message/result

![image](https://user-images.githubusercontent.com/100773863/162554295-a85ae624-0a7e-441a-8f0d-31a2c19faf95.png)


**We will rename the custom image (dockerimage:1) to desired one, here I rename it to:**

~~~sh
docker image tag dockerimage:1 devanandts/dockertestimage:custom
docker image tag devanandts/dockertestimage:custom devanandts/dockertestimage:latest
~~~

>Result

![image](https://user-images.githubusercontent.com/100773863/162554522-dba95987-9443-4b2b-8a62-22ed06915082.png)


Now, we can push the image to docker hub

~~~sh
docker image push devanandts/dockertestimage:custom
docker image push devanandts/dockertestimage:latest
~~~

>Result
>
![image](https://user-images.githubusercontent.com/100773863/162554612-963b1175-5b54-4577-abfa-34644ce47b34.png)
![image](https://user-images.githubusercontent.com/100773863/162554635-716ed820-76d4-47f1-b723-dc572dc4eabe.png)

Now we can pull this image for creating docker container!

## Conclusion

This is how an docker custom image is created and pushed to docker hub. Please contact me when you encounter any difficulty error while using this terrform code. Thank you and have a great day!


### ⚙️ Connect with Me
<p align="center">
<a href="https://www.instagram.com/dev_anand__/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/dev-anand-477898201/"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a>



