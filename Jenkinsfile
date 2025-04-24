pipeline {
  agent any
  environment {
    DOCKER_IMAGE = "queengarcia/stock-ms"
  }

  stages {
    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/queendgarcia/stock-ms.git'
      }
    }
    stage('Build & Test') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }
    stage('Docker Build & Push') {
      steps {
        script {
          // Ensure Docker BuildKit is enabled
          env.DOCKER_BUILDKIT = '1'

          // Build Docker image
          sh 'docker build -t $DOCKER_IMAGE:latest .'

          // Login to DockerHub
         withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
            sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
         }

          // Push the Docker image to DockerHub
          sh 'docker push $DOCKER_IMAGE:latest'
        }
      }
    }
  }
  
}
