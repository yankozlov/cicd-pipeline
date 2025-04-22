pipeline {
    agent any

    tools {
        nodejs "node-7.8.0"
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'   
            }
        }
        stage('Build Docker image') {
            steps {
                sh 'docker build -t node${GIT_BRANCH}:v1.0 .'
            }
        }
    }
}
