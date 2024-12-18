pipeline {
    agent any

 environment {
            
            Sonar_Server = 'Sonar13'
            NEXUS_CREDS = credentials('Nexus') // Nexus credential ID
        NEXUS_DOCKER_REPO = 'http://16.171.134.119:8082'

}


    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/ashwati13/docker-reactjs', branch: 'master'
                  }
        }
        
        
        
     stage('SonarQube Scan') {
            steps {
                script {
                    withSonarQubeEnv('Sonar13') { // Replace with your SonarQube server name
                        sh '''
                        # Install dependencies
                        npm install
                        
                        # Run SonarQube Scanner
                        npx sonar-scanner \
                            -Dsonar.projectKey=react-app
                        '''
                    }
                }
            }
        }
        
        
        stage('Run Tests') {
            steps {
                sh 'git --version'
            }
        }  
     
        
        
        stage('Nexus') {
            steps {
                echo 'Nexus Docker Repository Login'
                script{
                    withCredentials([usernamePassword(credentialsId: 'nexus_creds', usernameVariable: 'USER', passwordVariable: 'PASS' )]){
                       sh ' echo $PASS | docker login -u $USER --password-stdin 16.171.134.119:8082'
                    echo 'Building docker Image'
                    sh 'docker push ashwati13/docker_react-app'
                    }
                   
                }
            }
    } 
        
        
        stage('Deploy Application') {
            steps {

                sh 'sudo docker-compose up -d'
            }
        }

    }
    
}
  
