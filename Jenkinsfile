pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "garry984/fastapi-k8s:latest"
  }

  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/GauravRai1984/fastapi-k8s.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${DOCKER_IMAGE}")
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          script {
            sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
            sh "docker push ${DOCKER_IMAGE}"
          }
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        script {
          sh 'kubectl apply -f deployment.yaml'
          sh 'kubectl apply -f service.yaml'
        }
      }
    }
  }

  post {
    success {
      echo "Deployment to Kubernetes successful!"
    }
    failure {
      echo "Pipeline failed."
    }
  }
}
