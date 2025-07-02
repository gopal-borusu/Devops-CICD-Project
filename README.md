# Devops-CICD-Project
Apache Web Server setup with a CI/CD pipeline using Jenkins, Docker, Kubernetes, and AWS EC2.


## AWS Instance Creation
---
Launch your AWS console ---> EC2 ---> INSTANCES ---> Launch instances

Fill in all the required details and follow below mentioned recommendations as we will be going to use the same node for minikube installation (reference: [minikube documentation](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download))

  1) 2 CPUs or more
  2) 2GB of free memory
  3) 20GB of free disk space

Note: Allow HTTP and SSH Traffic

## Jenkins Installation
---
reference: [jenkins documentation](https://www.jenkins.io/doc/book/installing/linux/)

#### Commands to install
=============================

sudo apt update

sudo apt install fontconfig openjdk-21-jre -y

java -version

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update

sudo apt-get install jenkins -y

================================

Note: You need to access to port 8080 on which jenkins runs by default. Edit inbound traffic rules and add a rule to allow TCP traffic from port 8080.

Access jenkins UI at: serverip:8080

![image](https://github.com/user-attachments/assets/f15c2ed1-0a16-40b8-89c6-96dd2d3797c7)

![image](https://github.com/user-attachments/assets/a6c42bb8-9895-4fd3-bdca-64c69d6a74d8)

![image](https://github.com/user-attachments/assets/747b7b98-8379-4cae-96d2-eb2c1e8d9528)

![image](https://github.com/user-attachments/assets/f5d494d1-2adb-4e9a-a7b6-5a1c4d90953c)

![image](https://github.com/user-attachments/assets/901f790c-5fb8-48fb-96d4-27faf7b88f1e)



## Minikube and docker Installation
---

Reference: [minikube installation](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download)

#### Commands to install
=============================

sudo apt-get install docker.io -y

snap install kubectl --classic

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

minikube version

minikube start --driver=docker --force

kubectl get nodes

kubectl get pods

![image](https://github.com/user-attachments/assets/7d73ec2a-96bd-4672-8639-03be4d47caac)


================================








