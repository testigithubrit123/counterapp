pipeline {
    agent any

    tools {
        maven 'Maven 3.8.7'
    }

    environment {
        SONAR_SCANNER_HOME = tool 'SonarScanner'  // Register scanner path
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
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'TOKEN')]) {
                        sh '''
                            $SONAR_SCANNER_HOME/bin/sonar-scanner \
                            -Dsonar.projectKey=counterapp \
                            -Dsonar.sources=. \
                            -Dsonar.java.binaries=target/classes \
                            -Dsonar.login=$TOKEN
                        '''
                    }
                }
            }
        }
    }
}

