pipeline {
  agent any
   stages {
        stage('Build Docker Image') {
            steps {
                git branch: 'main', url: "https://github.com/gopal-borusu/Devops-CICD-Project"
                sh "docker build -t my-web-app:${BUILD_NUMBER} ."
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
