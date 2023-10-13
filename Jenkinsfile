/* groovylint-disable NglParseError */
/* groovylint-disable-next-line CompileStatic */
/* groovylint-disable-next-line CompileStatic, NglParseError */
pipeline {
    agent any //declaration globale de l'agent
    stages {
        stage('Cloning code') {
            steps {
                script {
                    sh '''
                        rm -rf mini-projet-jenkins || echo "Directory doesn't exists "
                        sleep 2
                        git clone https://github.com/gbaneassouman/mini-projet-jenkins.git
                    '''
                }
            }
        }
        stage('Build image') {
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString */
                    sh 'docker build -t ${DOCKER_ID}/${IMAGE_NAME}:${IMAGE_TAG} .'
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString */
                    sh '''
                        docker rm -f ${CONTAINER} || echo "container does not exist"
                        docker run --name ${CONTAINER} -d -p ${HOST_PORT}:${INTERNAL_PORT} ${IMAGE_NAME}
                        sleep 10
                        curl http://172.17.0.1:${HOST_PORT}
                    '''
                }
            }
        }
        stage('Release image') {
            environment {
                DOCKERHUB_PASSWORD  = credentials('dockerhub-credentials')
            }
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString */
                    sh '''
                        echo $DOCKERHUB_PASSWORD_PSW | docker login -u openlab89 --password-stdin
                        docker push ${DOCKER_ID}/${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }
        stage('Clean image') {
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString */
                    sh '''
                        docker rm -f ${CONTAINER}
                        docker rm -f ${IMAGE_NAME}
                    '''
                }
            }
        }
    }
}
