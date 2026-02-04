pipeline {
    agent any

    tools {
        nodejs 'node16'
    }

    environment {
        IMAGE_NAME = "node-todo-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/LondheShubham153/node-todo-cicd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('sonar') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=Node-Todo \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://34.236.152.163:9000
                        """
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy image $IMAGE_NAME'
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}

