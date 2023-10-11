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
                    sh 'git clone https://github.com/gbaneassouman/mini-projet-jenkins.git'
                }
            }
        }
        stage('Test image') {
            agent any
            steps {
                script {
                    sh 'echo Test'
                }
            }
        }
        stage('Release image') {
            agent any
            steps {
                script {
                    sh 'echo release'
                }
            }
        }
        stage('Clean image') {
            agent any
            steps {
                script {
                    sh 'echo clean'
                }
            }
        }
    }
}
