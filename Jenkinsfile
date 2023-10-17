/* groovylint-disable GStringExpressionWithinString, LineLength, NestedBlockDepth, NglParseError */
/* groovylint-disable-next-line CompileStatic */
/* groovylint-disable-next-line CompileStatic, NglParseError */
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
            // {environment {
            //     DOCKERHUB_PASSWORD  = credentials('dockerhub-credentials')
            // }
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
                    sh 'docker rm -f ${CONTAINER} || echo "container does not exist"'
                    sh 'docker run --name ${CONTAINER} -d -p ${HOST_PORT}:${INTERNAL_PORT} ${IMAGE_NAME}:${IMAGE_TAG}'
                    sh 'sleep 10'
                    sh 'curl http://172.17.0.1:${HOST_PORT}'
                }
            }
        }
        stage('Release image') {
            // environment {
            //     DOCKERHUB_PASSWORD  = credentials('dockerhub-credentials')
            // }
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString */
                    sh '''
                        docker image tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                        echo $DOCKERHUB_PASSWORD_PSW | docker login -u ${DOCKER_HUB} --password-stdin
                        docker push ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                        docker save $IMAGE_NAME:$IMAGE_TAG > /tmp/${ID_DOCKER}/$IMAGE_NAME:$IMAGE_TAG.tar
                    '''
                }
            }
        }
        stage('Clean image') {
            steps {
                script {
                /* groovylint-disable-next-line GStringExpressionWithinString */
                    sh '''
                    docker stop ${CONTAINER}
                    docker rm -f ${CONTAINER}
                    docker rmi -f ${IMAGE_NAME}
                    docker rmi -f ${IMAGE_NAME}:${IMAGE_TAG}
                    docker rmi -f ${DOCKER_HUB}/${IMAGE_NAME}
                    docker rmi -f ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                    kill $(ps aux | grep '5000' | awk '{print $2}')

                '''
                }
            }
        }
        stage('Deploy to Satging') {
            steps {
                script {
                    /* groovylint-disable-next-line GStringExpressionWithinString, NestedBlockDepth */
                    // sshagent(['SSH-KEY']) {
                    //     sh '''
                    //         ssh -o StrictHostKeyChecking=no -l ${USER_NAME} ${STAGING}
                    //         echo $DOCKERHUB_PASSWORD_PSW | docker login -u ${DOCKER_HUB} --password-stdin
                    //         docker pull ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                    //         docker run --name ${STAGING_NAME} -d -p ${HOST_PORT}:${INTERNAL_PORT} ${DOCKER_HUB}/${IMAGE_NAME}:${IMAGE_TAG}
                    //     '''
                    // }
                    sshPublisher(
                        publishers:
                            [sshPublisherDesc(configName: 'staging-srv',
                            transfers:
                                [sshTransfer(cleanRemote: false,
                                excludes: '',
                                execCommand: '''
                                    docker  load -i ${ID_DOCKER}/$IMAGE_NAME:$IMAGE_TAG.tar
                                    sleep 10
                                    /* groovylint-disable-next-line LineLength */
                                    docker run --name ${ID_DOCKER}/$IMAGE_NAME:$IMAGE_TAG -d -p ${HOST_PORT}:${INTERNAL_PORT} ${ID_DOCKER}/$IMAGE_NAME:$IMAGE_TAG
                                    sleep 5
                                    /* groovylint-disable-next-line LineLength */
                                    curl 172.17.0.1:${HOST_PORT}''',
                                    execTimeout: 120000,
                                    flatten: false,
                                    makeEmptyDirs: false,
                                    noDefaultExcludes: false,
                                    patternSeparator: '[, ]+',
                                    /* groovylint-disable-next-line LineLength */
                                    remoteDirectory: 'artifacts',
                                    remoteDirectorySDF: false,
                                    removePrefix: '',
                                    sourceFiles: '/tmp/${ID_DOCKER}/$IMAGE_NAME:$IMAGE_TAG.tar')],
                                    usePromotionTimestamp: false,
                                    useWorkspaceInPromotion: false,
                                    verbose: false)
                            ]
                        )
                }
            }
        }
    }
}
// }

