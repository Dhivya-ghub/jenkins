pipeline {
    agent any
    stages {
        stage('git clone') {
            steps {
                // Get code from a GitHub repository
                git url: 'https://github.com/Dhivya-ghub/jenkins.git', branch: 'feature',
                 credentialsId: 'github_creds'
            }
        }
        stage('Docker Build and Tag') {
           steps {
               bat "docker build -t dhivyadhub/pydocker1:%BUILD_NUMBER% ." 
            }  
        }
        stage('Run Docker container') {
          steps {
                bat "docker run -d --name pythoncon -p 5008:5000 dhivyadhub/pydocker1:%BUILD_NUMBER%"
            }
        }
        stage('Docker Testing') {
          steps {
                bat "http://localhost:%BUILD_NUMBER%"
            }
        }
        stage('DockerHub login and push the docker image') {
          steps {
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR')]) {
                bat 'docker push dhivyadhub/pydocker1:%BUILD_NUMBER%'
               }
           }
        }    
    }            
}
