/* groovylint-disable-next-line CompileStatic */
pipeline {
    environment { 
        IMAGE_NAME = ""
        IMAGE_TAG = "latest"
        INTERNAL_PORT = ""
        DOCKER_ID = ""
        CONTAINER_IMAGE = ${DOCKER_ID}/${IMAGE_NAME}:${IMAGE_TAG}
    }
    agent none
    stages {
        stage('Build image') {
            agent any
            steps {
                script {
                    sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
                }
            }
        }
        stage('Test image') {
            agent any
            steps {
                script {
                    sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
                }
            }
        }
        stage('Release image') {
            agent any
            steps {
                script {
                    sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
                }
            }
        }
        stage('Clean image') {
            agent any
            steps {
                script {
                    sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
                }
            }
        }
    }
}
