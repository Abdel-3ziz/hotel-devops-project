pipeline {
  agent any

  environment {
    APP_NAME = "hotel-app"
    IMAGE    = "hotel-app:latest"
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t ${IMAGE} -f Hotel/Dockerfile Hotel'
      }
    }

    stage('Deploy (Docker on EC2)') {
      steps {
        sh '''
          set -eux
          docker rm -f hotel-app || true
          docker run -d --name hotel-app --restart unless-stopped -p 80:80 hotel-app:latest
        '''
      }
    }

    stage('Smoke Check') {
      steps {
        sh '''
          set -eux
          docker ps --filter "name=hotel-app"
          curl -I --max-time 10 http://localhost/ || true
        '''
      }
    }
  }

  post {
    always {
      sh 'docker ps | head -n 20'
      sh 'docker logs --tail 100 hotel-app || true'
    }
  }
}
