pipeline {
    agent any
       tools{
       maven 'Maven3'
       }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/anjalitssl/POC10.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_SCANNER_HOME = tool 'sonar-scanner'
            }
            steps {
                sh """
                    ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=poc10 \
                    -Dsonar.sources=src \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.login=squ_a213a2faf4c32e6e45fd58efec581e7ac2ec9796
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t poc10-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8085:8080 --name poc10-container poc10-app || true'
            }
        }
    }
}