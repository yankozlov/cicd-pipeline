pipeline {
    agent any

    tools {
        nodejs "node-7.8.0"
    }

    environment {
        IMAGE_NAME  = "node${env.BRANCH_NAME}"
        IMAGE_TAG   = 'v1.0'
        PORT        = "${env.BRANCH_NAME == 'dev' ? 3000 : 3001}"
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
                sh 'docker run -d --expose 3000 -p ${PORT}:3000 ${IMAGE_NAME}:${IMAGE_TAG}'
            }
        }
    }
}
