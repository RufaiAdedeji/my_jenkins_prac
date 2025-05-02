pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "rufaicloudpractice1/chucknorrisapp"
        DOCKERFILE = "Dockerfile"
        DOCKER_REGISTRY = "index.docker.io/v1"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
        EC2_USER = "ubuntu"
        EC2_HOST = ""
        SSH_CREDENTIALS_ID = "ec2-ssh-key"
        CONTAINER_NAME = "chucknorris-app"
        HOST_PORT = 80
        CONTAINER_PORT = 3000
    }
    stages {
        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/RufaiAdedeji/my_jenkins_prac.git', branch: 'main'
            }
        }

        stage('Install Dependences') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Test') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building docker image'
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}", "-f ${DOCKERFILE} .")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'pushing docker image to docker hub'
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()    
                }
            }
        }

        stage('Deploy') {

        }
    }
}