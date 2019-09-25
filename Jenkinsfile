pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'toto1310/simple-jenkins-dood'
        DOCKER_ALIAS = 'toto1310/jenkins-docker'
    }
    stages {
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                timestamps {
                    retry(3) {
                        sh label: 'image build', script: '''
                            export DOCKER_TAG=${JOB_NAME}
                            if [ -z ${DOCKER_TAG##*alpine*} ]; then
                                export DOCKER_FILE="Dockerfile-alpine"
                            else
                                export DOCKER_FILE="Dockerfile"
                            fi
                            docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} --build-arg tag=${DOCKER_TAG} --file ${DOCKER_FILE} --pull ${DOCKER_BUILD_OPS} .
                        '''
                    }
                }
            }
        }
        stage('Push') {
            when {
                environment name: 'DOCKER_PUSH', value: 'true'
            }
            steps {
                timestamps {
                    retry(3) {
                        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_HUB_PASS', usernameVariable: 'DOCKER_HUB_USER')]) {
                            sh label: 'docker login', script: '''
                                docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASS} https://index.docker.io/v1/
                            '''
                        }
                    }
                    retry(3) {
                        sh label: 'image push', script: '''
                            docker push ${DOCKER_IMAGE}:${JOB_NAME}
                            docker tag ${DOCKER_IMAGE}:${JOB_NAME} ${DOCKER_ALIAS}:${JOB_NAME}
                            docker push ${DOCKER_ALIAS}:${JOB_NAME}
                        '''
                    }
                }
            }
        }
    }
    post {
        always {
            emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:

Check console output at $BUILD_URL to view the results.
=========
${BUILD_LOG, maxLines=200, escapeHtml=false}
''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: '$DEFAULT_RECIPIENTS'
        }
    }
}