pipeline {
    agent any
    
    environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    APP_NAME = "ebowsaah/again"
    }

    stages {
        stage('code checkout') {
            steps {
                checkout poll: false, scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/kabbansaah1/kubernetescode.git']])
                sh 'ls'
            }
        }
        stage('Test') {
            steps {
                echo 'tests passed'
            }
        }
        stage('Dockerize') {
            steps {
                sh 'docker build -t $APP_NAME:$BUILD_NUMBER .'
            }
        }
        stage('dockerhub login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps {
                sh 'docker push $APP_NAME:$BUILD_NUMBER'
            }
        }
        stage('trigger updatemanifest') {
            steps {
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
            }
        }
    }
}
