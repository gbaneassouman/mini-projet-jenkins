/* groovylint-disable GStringExpressionWithinString, NglParseError */
/* groovylint-disable-next-line CompileStatic */
/* groovylint-disable-next-line CompileStatic, NglParseError */
pipeline {
    // environment {
    //     ${DOCKER_ID}/${IMAGE_NAME}:${IMAGE_TAG}
    // }
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
        // stage('Build image') {
        //     steps {
        //         script {
        //             /* groovylint-disable-next-line GStringExpressionWithinString */
        //             sh '''
        //                 docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
        //             '''
        //         }
        //     }
        // }
        // stage('Test image') {
        //     steps {
        //         script {
        //             /* groovylint-disable-next-line GStringExpressionWithinString */
        //             sh 'docker rm -f ${CONTAINER} || echo "container does not exist"'
        //             sh 'docker run --name ${CONTAINER} -d -p ${HOST_PORT}:${INTERNAL_PORT} ${IMAGE_NAME}:${IMAGE_TAG}'
        //             sh 'sleep 10'
        //             sh 'curl http://172.17.0.1:${HOST_PORT}|grep -q "Dimension"'
        //         }
        //     }
        // }
        // stage('Release image') {
        //     environment {
        //         DOCKERHUB_PASSWORD  = credentials('dockerhub-credentials')
        //     }
        //     steps {
        //         script {
        //             /* groovylint-disable-next-line GStringExpressionWithinString */
        //             sh '''
        //                 docker image tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
        //                 echo $DOCKERHUB_PASSWORD_PSW | docker login -u openlab89 --password-stdin
        //                 docker push ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
        //             '''
        //         }
        //     }
        // }
        stage('SSH') {
            steps {
                sshagent(['SSH-KEY']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no -l admin ${STAGING} uname -a
                '''
                }

            }
        }
        // stage('Deploy to Satging') {
        //     // environment {
        //     //     DOCKERHUB_PASSWORD  = credentials('dockerhub-credentials')
        //     // }
        //     steps {
        //         script {
        //             /* groovylint-disable-next-line GStringExpressionWithinString */
        //             sh '''
        //                 ssh -i ${SSH_KEY} admin@${STAGING}
        //             '''
        //         }
        //     }
        // }
        // stage('Clean image') {
        //     steps {
        //         script {
        //             /* groovylint-disable-next-line GStringExpressionWithinString */
        //             sh '''
        //                 docker stop ${CONTAINER}
        //                 docker rm -f ${CONTAINER}
        //                 docker rmi -f ${IMAGE_NAME}
        //             '''
        //         }
        //     }
        // }
        }
    }
