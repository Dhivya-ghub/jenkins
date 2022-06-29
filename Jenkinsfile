pipeline {
    agent any
    environment {
        registry = "dhivyadhub/pydocker1"    
    }
    stages {
        stage('git clone') {
            steps {
                // Get code from a GitHub repository
                git url: 'https://github.com/Dhivya-ghub/jenkins.git', branch: 'main',
                 credentialsId: 'github_creds'
            }
        }
        stage('Docker Build and Tag') {
           steps {
                 bat "docker build -t %registry%:%BUILD_NUMBER% ." 
           }  
         }
         stage('Delete the unwanted Docker container') {
           steps {
                 bat '''
                    FOR /f "tokens=*" %i IN ('docker ps -aq') DO docker rm -f %i
                 '''
           }
         }
        stage('DockerHub login and push the docker image') {
          steps {
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR')]) {
                bat 'docker login -u %DOCKERHUB_CREDENTIALS_USR% -p %DOCKERHUB_CREDENTIALS_PSW% && docker push %registry%:%BUILD_NUMBER%'
               }
           }
         }    
        stage('Run Docker container') {
          steps {
                bat "docker run -d --name pythoncon%BUILD_NUMBER% -p 50%BUILD_NUMBER%:5000 --restart unless-stopped dhivyadhub/pydocker1:%BUILD_NUMBER%"
          }
        }
    }   
}
