pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "hassejaharsha-cell/nodejs-jenkins-demo"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/hassejaharsha-cell/nodejs-jenkins-cicd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                credentialsId: 'dockerhub-creds',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
                )]) {

                sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop nodeapp || true
                docker rm nodeapp || true
                docker run -d -p 80:3000 --name nodeapp $DOCKER_IMAGE:latest
                '''
            }
        }

    }
}
