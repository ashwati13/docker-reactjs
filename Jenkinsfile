pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('Docker') 
        DOCKER_IMAGE = 'ashwati13/react-app'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/ashwati13/docker-reactjs', branch: 'master'
                  }
        }

        stage('Run Tests') {
            steps {
                sh 'git --version'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKERHUB_CREDENTIALS') {
                        sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    }
                }
            }
        }
        stage('Deploy Application') {
            steps {

                sh 'pwd'
                sh 'cd docker-reactjs/'
                sh '/usr/local/lib/docker/cli-plugins/docker-compose up -d'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
