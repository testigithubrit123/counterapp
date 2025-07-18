pipeline {
    agent any

    environment {
        PROJECT_ID = 'your-gcp-project-id'
        CLUSTER = 'your-cluster-name'
        ZONE = 'us-central1-a'
        DOCKER_IMAGE = 'dockerhub-username/counterapp'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/testigithubrit123/counterapp.git'
            }
        }

        stage('Auth GCP') {
            steps {
                withCredentials([file(credentialsId: 'gcp-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                      gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                      gcloud config set project $PROJECT_ID
                      gcloud container clusters get-credentials $CLUSTER --zone $ZONE
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                        sh "docker push ${DOCKER_IMAGE}:latest"
                    }
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                '''
            }
        }
    }
}



