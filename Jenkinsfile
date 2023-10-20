/* groovylint-disable NglParseError */
/* groovylint-disable DuplicateListLiteral, DuplicateStringLiteral, GStringExpressionWithinString, LineLength, NestedBlockDepth, NglParseError */
/* groovylint-disable-next-line CompileStatic */
/* groovylint-disable-next-line CompileStatic, NglParseError */
/* import shared library */
// @library('shared-library')
/* groovylint-disable-next-line CompileStatic */
@Library('slack-shared-library') _

pipeline {
    environment {
        DOCKERHUB_PASSWORD  = credentials('dockerhub-credentials')
    }
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
                    sh '''
                        docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                    '''
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString */
                    // sh 'docker stop ${CONTAINER}|| echo "already stopped"'
                    // sh 'docker rm -f ${CONTAINER} || echo "container does not exist"'
                    sh 'docker run --name ${CONTAINER} -d -p ${HOST_PORT}:${INTERNAL_PORT} ${IMAGE_NAME}:${IMAGE_TAG}'
                    sh 'sleep 10'
                    sh 'curl -k http://172.17.0.1:${HOST_PORT}|grep -i "DIMENSION"'
                }
            }
        }
        stage('Release image') {
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString */
                    sh '''
                        docker save ${IMAGE_NAME}:${IMAGE_TAG} > /tmp/${IMAGE_NAME}:${IMAGE_TAG}.tar
                        docker image tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                        echo $DOCKERHUB_PASSWORD_PSW | docker login -u ${DOCKER_HUB} --password-stdin
                        docker push ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }
        stage('Clean image') {
            steps {
                script {
                /* groovylint-disable-next-line GStringExpressionWithinString */
                    //ps aux  |  grep -i 5000  |  awk '{print $2}'  |  xargs sudo kill -9
                    //kill $(ps aux | grep '5000' | awk '{print $2}')
                    sh '''
                    docker stop ${CONTAINER}
                    docker rm -f ${CONTAINER}
                    docker rmi -f ${IMAGE_NAME}
                    docker rmi -f ${DOCKER_HUB}/${IMAGE_NAME}
                '''
                }
            }
        }
        stage('Deploy to Satging') {
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString, NestedBlockDepth */
                    sshagent(['SSH-KEY']) {
                        sh '''
                            echo $DOCKERHUB_PASSWORD_PSW | docker login -u ${DOCKER_HUB} --password-stdin
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${STAGING} docker stop ${STAGING_NAME}||echo "stopped"
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${STAGING} docker rm ${STAGING_NAME}||echo "already deleted"
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${STAGING} docker pull ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                            sleep 120
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${STAGING} docker run --name ${STAGING_NAME} -d -p ${HOST_PORT}:${INTERNAL_PORT} ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${STAGING} curl -k http://172.17.0.1:${HOST_PORT}|grep -i "DIMENSION"
                        '''
                    }
                }
            }
        }
        stage('Test in staging') {
            steps {
                script {
                    sshagent(['SSH-KEY']) {
                        sh '''
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${STAGING} curl -k http://172.17.0.1:${HOST_PORT}|grep -i "DIMENSION"
                        '''
                    }
                }
            }
        }
        stage('Deploy to Prod') {
            // when {
            //     env.BRANCH_NAME == 'main'
            // }
            when {
                branch 'main'
            }
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString, NestedBlockDepth */
                    sshagent(['prod-area']) {
                        sh '''
                            echo $DOCKERHUB_PASSWORD_PSW | docker login -u ${DOCKER_HUB} --password-stdin
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${STAGING} docker stop ${PROD_NAME}||echo "stopped"
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${STAGING} docker rm ${PROD_NAME}||echo "already deleted"
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${PROD} docker pull ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                            sleep 120
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${PROD} docker run --name ${PROD_NAME} -d -p ${HOST_PORT}:${INTERNAL_PORT} ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                            ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${PROD} curl -k http://172.17.0.1:${HOST_PORT}|grep -i "DIMENSION"
                        '''
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                /* Use Slack-notification.groovy from shared library */
                slackNotifier
            }
        }
    }
}
// }

