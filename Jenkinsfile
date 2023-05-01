pipeline {
    agent any
    environment {
        DOCKERHUB_CRED = credentials('jenkins-dockerhub')
    }

    stages {
        stage('SCM Checkout') {
            steps {
                git 'https://github.com/roman-polonsky-22/UnderTest.git'
            }
        }
        stage('Build docker image') {
            steps {
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
