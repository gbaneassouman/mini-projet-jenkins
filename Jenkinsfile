/* groovylint-disable NglParseError */
/* groovylint-disable-next-line CompileStatic */
/* groovylint-disable-next-line CompileStatic, NglParseError */
pipeline {
    agent any //declaration globale de l'agent
    stages {
        stage('Cloning code') {
            steps {
                script {
                    sh 'git clone https://github.com/gbaneassouman/mini-projet-jenkins.git'
                }
            }
        }
        stage('Build image') {
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString */
                    sh 'docker build -t ${IMAGE_NAME} .'
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    sh 'echo Test'
                }
            }
        }
        stage('Release image') {
            steps {
                script {
                    sh 'echo release'
                }
            }
        }
        stage('Clean image') {
            steps {
                script {
                    sh '''
                    echo clean
                    rm -rf 
                    '''
                }
            }
        }
    }
}
