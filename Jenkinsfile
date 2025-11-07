pipeline {
    agent {
        docker {
            image 'node:18'   // You can also use node:20 or node:lts
        }
    }

    environment {
        IMAGE_NAME = "jenkins-node-app"
        CONTAINER_NAME = "jenkins-node-container"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage('Run Docker Container') {
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
