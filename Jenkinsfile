pipeline {
    agent any

    tools {
        maven 'Maven 3.8.7'
    }

    environment {
        SONARQUBE_SCANNER = tool 'SonarScanner'
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
                    withCredentials([string(credentialsId: 'sonar-token1', variable: 'SONAR_TOKEN')]) {
                        sh """
                            ${SONARQUBE_SCANNER}/bin/sonar-scanner \
                            -Dsonar.projectKey=counterapp \
                            -Dsonar.sources=. \
                            -Dsonar.java.binaries=target/classes \
                            -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }
    }
}


