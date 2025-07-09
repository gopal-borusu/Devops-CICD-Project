# Devops-CICD-Project
Apache Web Server setup with a CI/CD pipeline using Jenkins, Docker, Kubernetes, and AWS EC2.

![AWS EC2 Instance](https://github.com/user-attachments/assets/83e1f581-bf7a-46f8-ad21-ab103a2c570e)



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

sudo su - jenkins

minikube version

minikube start --driver=docker --force --ports=32767:32767   #(Starting minikube as jenkins user and we are mapping our server port 32767 with minikube port 32767 to acess the application once built)

kubectl get nodes

kubectl get pods

![image](https://github.com/user-attachments/assets/7d73ec2a-96bd-4672-8639-03be4d47caac)


================================


## Jenkins plugins installation & credentials configuration
---

1) add jenkins user to docker group  ----> sudo usermod -aG docker jenkins;id jenkins
2) Go to jenkins UI ---> Manage Jenkins --> Plugins ---> Available Plugins, and install below plugins
   
     (i) Docker Pipeline
3) restart the jenkins service -----> sudo systemctl restart jenkins
4) Go to jenkins UI ---> Manage Jenkins --> Credentials --> click global --> Add Credentials

  Give Your Docker Hub Credentials (Do not change the name of ID) and click on create
  
  ![image](https://github.com/user-attachments/assets/7689a307-bf4f-4c5d-a396-fefce704b341)

5) Click again on add credentials and give below mentioned details. (You should upload the contents of kubeconfig file here which can be found at cat ~/.kube/config)

![image](https://github.com/user-attachments/assets/7ec573f8-f44b-40eb-a440-6c6fe2a78755)



## Jenkins Pipeline creation
---

1) Go to Jenkins home page--> New item, provide below details and click ok

     i) Name: anything
     ii) item type: Pipeline  
2) Provide Description if you want and scroll down to pipeline section
3) In Triggers select "GitHub hook trigger for GITScm polling" option.
4) In Definition select "pipeline from SCM" and SCM as "GIT"
5) Provide the repository URL (in my case it is https://github.com/gopal-borusu/Devops-CICD-Project)
6) In branch specifier select branch name. (in my case it is main)
7) In Scriptpath provide the name of the jenkinsfile as present in your github (in my case it is Jenkinsfile)
8) Leave everything else as default and click on save.
9) You can see the pipeline got created.
10) Enable webhook in the github repository by going to repository --> settings --> webhhok --> add webhook
  
      i) give repository url as your jenkins url
    
      ii) content type "application/json"
    
      iii) Which events would you like to trigger this webhook? ---> Just the push event
    
      iv) chackmark the Active box and click Add Webhook.

---

We have completed configuring our CI/CD pipeline. If we commit any changes to the repo then it will trigger the pipeline automatically which in turn will create new docker image and push it to docker hub. The new image will then be used to deploy to kubernetes.


## Testing

Let us make our first commit to github branch and see whether the jenkins pipeline is getting triggered.

commit screenshot

jenkins screenshot

docker hub screenshot

kubernetes pod screenshot

website screenshot


Now, let us make one more commit and see whther the new changes are reflecting or not.

commit screenshot

jenkins screenshot

docker hub screenshot

kubernetes pod screenshot

website screenshot


As we can see, whenever a new commit is done to the github branch it is automatically triggering the jenkins pipeline and executing the required actions.




