pipeline {
    agent any


    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/ashwati13/docker-reactjs', branch: 'master'
                  }
        }
        
        
        stage('Deploy') {
          steps {
              withCredentials([usernamePassword(credentialsId: "${DOCKER_CRED}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                 sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
                  sh 'sudo docker push ashwati13/docker_react-app'
        }
      }
    } 
        
        
        
        
        stage('Run Tests') {
            steps {
                sh 'git --version'
            }
        }
        
        stage('Deploy Application') {
            steps {

                sh 'sudo docker-compose up -d'
            }
        }

    }
    
}
