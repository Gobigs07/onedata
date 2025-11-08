pipeline {
    agent none

    environment {
        IMAGE_NAME = "jenkins-node-app"
        CONTAINER_NAME = "jenkins-node-container"
    }

    stages {
        stage('Build & Test Node App') {
            agent {
                docker {
                    image 'node:18'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                checkout scm
                sh 'npm install'
                sh 'npm test || true'
            }
        }

        stage('Build Docker Image') {
            agent any
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage('Run Docker Container') {
            agent any
            steps {
                script {
                    sh '''
                        docker rm -f $CONTAINER_NAME || true
                        docker run -d --name $CONTAINER_NAME -p 3000:3000 $IMAGE_NAME
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build and Deployment Successful!"
        }
        failure {
            echo "❌ Build or Deployment Failed!"
        }
    }
}
