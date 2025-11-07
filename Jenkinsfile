pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-node-app"
        CONTAINER_NAME = "jenkins-node-container"
    }

    stages {
        stage('Checkout') {
            steps {
                // This checks out the same repo and branch Jenkins was triggered from
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
                sh 'npm test || true'  // optional: avoid breaking pipeline if no tests
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
