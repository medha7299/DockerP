pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds' // The ID you'll set in Jenkins
        APP_NAME = "medha7299/flask-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${APP_NAME}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Login and Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKERHUB_CREDENTIALS}",
                    passwordVariable: 'PASS',
                    usernameVariable: 'USER'
                )]) {

                    sh "echo ${PASS} | docker login -u ${USER} --password-stdin"

                    sh "docker push ${APP_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }
}
