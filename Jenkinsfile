pipeline {
    agent any

    environment {
        IMAGE_NAME = "calc-app"
        CONTAINER_NAME = "calc-container"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm  // If needed, I’ll help add GitHub credentials block
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Manual Approval to Deploy') {
            steps {
                input message: "🚀 Ready to deploy the app?", ok: "Yes, Deploy"
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop ${CONTAINER_NAME} || true'
                sh 'docker rm ${CONTAINER_NAME} || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}'
            }
        }
    }

    post {
        success {
            echo '✅ Build & Deploy Successful!'
            mail to: 'vishnusrighanta143@gmail.com',
                 subject: "✅ SUCCESS: Jenkins Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """Hi Vishnu 👋

The Jenkins job "${env.JOB_NAME}" (Build #${env.BUILD_NUMBER}) completed SUCCESSFULLY ✅

👉 Check it here: ${env.BUILD_URL}

Regards,  
Jenkins Bot 🤖"""
        }

        failure {
            echo '❌ Build or Deployment FAILED!'
            mail to: 'vishnusrighanta143@gmail.com',
                 subject: "❌ FAILURE: Jenkins Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """Hi Vishnu,

The Jenkins job "${env.JOB_NAME}" (Build #${env.BUILD_NUMBER}) FAILED ❌

👉 Check it here: ${env.BUILD_URL}

Please review the logs.

Regards,  
Jenkins Bot 🤖"""
        }

        always {
            echo '📦 Jenkins Pipeline Finished (success or fail).'
        }
    }
}

