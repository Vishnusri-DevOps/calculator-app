pipeline {
    agent any

    environment {
        IMAGE_NAME = "calc-app"
        CONTAINER_NAME = "calc-container"
        EMAIL = 'vishnusrighanta143@gmail.com'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${IMAGE_NAME} .
                """
            }
        }

        stage('Manual Approval') {
            steps {
                input message: "Do you want to deploy this Calculator App?", ok: "Yes, Deploy"
            }
        }

        stage('Stop Old Container') {
            steps {
                sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                """
            }
        }

        stage('Deploy New Container') {
            steps {
                sh """
                    docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                """
            }
        }
    }

    post {
        success {
            mail to: "${EMAIL}",
                 subject: "✅ SUCCESS: Jenkins Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Your Calculator App was successfully deployed on EC2! 🚀\n\n🔗 ${env.BUILD_URL}"
        }
        failure {
            mail to: "${EMAIL}",
                 subject: "❌ FAILURE: Jenkins Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Deployment failed. Please check Jenkins logs:\n\n🔗 ${env.BUILD_URL}"
        }
    }
}

