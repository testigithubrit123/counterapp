pipeline {
  agent any

  environment {
    PROJECT_ID = 'sunny-cider-459704-j1'
    REGION = 'us-central1'
    CLUSTER_NAME = 'demok8cluster'
    REPO = 'us-central1-docker.pkg.dev/sunny-cider-459704-j1/democlass2/app'
    IMAGE_TAG = '1.0'
    SERVICE_ACCOUNT_KEY = '/home/jenkins/jenkins-artifact.json'
  }

  stages {
    stage('Checkout Code') {
      steps {
        checkout scm
      }
    }

    stage('Build App') {
      steps {
        sh 'mvn clean install'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          docker build -t app:${IMAGE_TAG} .
          docker tag app:${IMAGE_TAG} ${REPO}:${IMAGE_TAG}
        '''
      }
    }

    stage('Authenticate to Google Cloud') {
      steps {
        sh '''
          gcloud auth activate-service-account --key-file=${SERVICE_ACCOUNT_KEY}
          gcloud auth configure-docker ${REGION}-docker.pkg.dev --quiet
          gcloud container clusters get-credentials ${CLUSTER_NAME} --region ${REGION} --project ${PROJECT_ID}
        '''
      }
    }

    stage('Push Docker Image') {
      steps {
        sh "docker push ${REPO}:${IMAGE_TAG}"
      }
    }

    stage('Deploy to GKE') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
      }
    }
  }

  post {
    failure {
      echo '❌ Build failed!'
    }
    success {
      echo '✅ Deployed successfully to GKE!'
    }
  }
}
