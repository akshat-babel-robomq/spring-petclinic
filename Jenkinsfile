pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    environment {
        SONAR_PROJECT_KEY = 'petclinic'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh './mvnw clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh './mvnw sonar:sonar -Dsonar.projectKey=${SONAR_PROJECT_KEY} -Dsonar.java.binaries=target'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t petclinic-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker rm -f petclinic-app || true'
                sh 'docker run -d -p 8082:8080 --name petclinic-app petclinic-app'
            }
        }
    }
}

