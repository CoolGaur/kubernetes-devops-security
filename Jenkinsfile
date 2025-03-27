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
      stage('SonarQube Analysis') {
            steps {
              withSonarQubeEnv() {
                sh "mvn clean verify sonar:sonar -Dsonar.projectKey=Numeric-Application -Dsonar.projectName='Numeric-Application'"
    }
  }
      stage('Docker Build and Push') {
            steps {
              withDockerRegistry([credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/']) {
                  sh "docker build -t kubernetes-devops-security ."
                  sh "docker tag kubernetes-devops-security coolgaur/maven-wrapper:kubeDevopsSec"
                  sh "docker push coolgaur/maven-wrapper:kubeDevopsSec"
              }
            }
        } 
      stage('Deploy to Kubernetes') {
            steps {
              withKubeConfig([credentialsId: 'kubeconfig']) {
                  sh "kubectl apply -f k8s_deployment_service.yaml"
              }
            }
        } 
    }
}