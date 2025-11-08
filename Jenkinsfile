pipeline {
    agent none

    environment {
        IMAGE_NAME = "jenkins-node-app"
        CONTAINER_NAME = "jenkins-node-container"
        EMAIL_RECIPIENTS = "gobinath.v1303@gmail.com"
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
            emailext(
                to: "${EMAIL_RECIPIENTS}",
                subject: "✅ Jenkins Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h3>🎉 Build Successful!</h3>
                <p>Job: <b>${env.JOB_NAME}</b></p>
                <p>Build Number: <b>${env.BUILD_NUMBER}</b></p>
                <p>Status: <b>SUCCESS</b></p>
                <p>View details: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                mimeType: 'text/html'
            )
        }

        failure {
            echo "❌ Build or Deployment Failed!"
            emailext(
                to: "${EMAIL_RECIPIENTS}",
                subject: "❌ Jenkins Build FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h3>🚨 Build Failed!</h3>
                <p>Job: <b>${env.JOB_NAME}</b></p>
                <p>Build Number: <b>${env.BUILD_NUMBER}</b></p>
                <p>Status: <b>FAILURE</b></p>
                <p>Check console output: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                mimeType: 'text/html'
            )
        }
    }
}
