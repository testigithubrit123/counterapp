pipeline {
    agent any

    tools {
        maven 'Maven 3.8.7'
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token') // Add this in Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/testigithubrit123/counterapp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}

