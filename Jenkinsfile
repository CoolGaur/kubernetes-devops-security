pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar'
            }
        }
      stage('Unit Testing') {
            steps {
              sh "mvn test"
            }
        } 
      stage('Docker Build and Push') {
            steps {
              docker.withRegistry('https://index.docker.io/v1/', 'docker-hub') {
                  sh "docker build -t kubernetes-devops-security ."
                  sh "docker tag kubernetes-devops-security coolgaur/maven-wrapper:kubeDevopsSec"
                  sh "docker push coolgaur/maven-wrapper:kubeDevopsSec"
              }
            }
        } 
      stage('Deploy to Kubernetes') {
            steps {
              sh "kubectl apply -f kubernetes-deployment.yaml"
            }
        } 
    }
}