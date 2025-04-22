pipeline {
    agent any

    tools {
        nodejs "node-7.8.0"
    }

    environment {
        DOCKER_CREDENTIALS_ID   = 'DockerHub'
        IMAGE_NAME      = "node${env.BRANCH_NAME}"
        IMAGE_TAG       = 'v1.0'
        CONTAINER_NAME  = "cicd-pipeline_${env.IMAGE_NAME}"
        PORT            = "${env.BRANCH_NAME == 'dev' ? 3001 : 3000}"
    }

    stages {
        stage('Build') {
            steps {
                sh 'bash scripts/build.sh'
            }
        }
        stage('Test') {
            steps {
                sh 'bash scripts/test.sh'
            }
        }
        stage('Login to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                        env.IMAGE_FULL_NAME = "${DOCKER_USERNAME}/${IMAGE_NAME}"
                    }
                }
            }
        }
        stage('Build Docker image') {
            steps {
                sh 'docker build -t ${IMAGE_FULL_NAME}:${IMAGE_TAG} .'
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${IMAGE_FULL_NAME}:${IMAGE_TAG}"
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '(docker stop ${CONTAINER_NAME} && docker rm ${CONTAINER_NAME}) || exit 0'
                sh 'docker run -d --name ${CONTAINER_NAME} --expose 3000 -p ${PORT}:3000 ${IMAGE_FULL_NAME}:${IMAGE_TAG}'
            }
        }
    }
}
