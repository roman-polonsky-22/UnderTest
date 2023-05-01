pipeline {
    agent any
    environment {
        DOCKERHUB_CRED = credentials('jenkins-dockerhub')
    }

    stages {
        
        stage('Build docker image') {
            steps {
                cd 'UnderTest'
                sh 'docker build -t rompolo/undertest:$BUILD_NUMBER .'
            }
        }
        stage('Login to dockerhub') {
            steps {
                sh 'echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin'
            }
        }
        stage('Push image') {
            steps {
                sh 'docker push rompolo/undertest:$BUILD_NUMBER'
            }
        }
    }
}
