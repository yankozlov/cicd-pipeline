pipeline {
    agent any

    tools {
        nodejs "node-7.8.0"
    }

    environment {
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
        stage('Build Docker image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }
        stage('Deploy') {
            steps {
                sh '(docker stop ${CONTAINER_NAME} && docker rm ${CONTAINER_NAME}) || exit 0'
                sh 'docker run -d --name ${CONTAINER_NAME} --expose 3000 -p ${PORT}:3000 ${IMAGE_NAME}:${IMAGE_TAG}'
            }
        }
    }
}
