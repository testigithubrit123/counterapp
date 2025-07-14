pipeline {
    agent any

    tools {
        maven 'Maven 3.8.7'
    }

    environment {
        SONARQUBE_SCANNER = 'SonarScanner'
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/testigithubrit123/counterapp', branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('My SonarQube Server') {
                    sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=counterapp \
                        -Dsonar.host.url=http://<SONARQUBE-IP>:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }
    }
}
