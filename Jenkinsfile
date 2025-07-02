pipeline {
  agent any
        environment {
          DOCKER_IMAGE = "gopalborusu16/my-web-app:${BUILD_NUMBER}"
          Github_Username = "gopal-borusu"
          Github_Repo = "Devops-CICD-Project"
          DOCKER_CREDENTIALS = credentials('docker_cred')
          KUBECONFIG = credentials('my-kubeconfig')
        }
   stages {
        stage('Build Docker Image') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: "https://github.com/${env.Github_Username}/${env.Github_Repo}"
                sh "docker build -t ${env.DOCKER_IMAGE} ."
            }
        }
        stage("Push Docker Image to docker hub") {
            steps {
              script {
                docker.withRegistry('https://index.docker.io/v1/', "docker_cred") {
                    def webapp_image = docker.image("${env.DOCKER_IMAGE}")
                    webapp_image.push()
                    webapp_image.push("latest")
                  }
                }
             }
          }
        stage("Deploy to kubernetes") {
            steps {
                sh 'sed -i "s/:{imagetag}/:${BUILD_NUMBER}/g" deployment.yaml'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
