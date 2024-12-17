pipeline {
    agent any

    tools {
    git 'Default'
}
    environment {
        DOCKERHUB_CREDENTIALS = credentials('Dockerhub') // Jenkins credential ID
        DOCKER_IMAGE = 'ashwati13/react-app'
        SONARQUBE_SERVER = 'Sonar' // SonarQube server name in Jenkins
        NEXUS_CREDENTIALS = credentials('Nexus') // Nexus credential ID
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh 'npm install'
                    sh 'sonar-scanner'
                }
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test -- --coverage'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Push to Nexus') {
            steps {
                script {
                    sh """
                    docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} 13.53.41.254:8081/repository/react_app/${DOCKER_IMAGE}:${BUILD_NUMBER}
                    docker login -u ${NEXUS_CREDENTIALS_USR} -p ${NEXUS_CREDENTIALS_PSW} nexus.example.com
                    docker push nexus.example.com/repository/docker/${DOCKER_IMAGE}:${BUILD_NUMBER}
                    """
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
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
