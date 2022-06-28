pipeline {
    agent any
    environment {
        registry = "dhivyadhub/pydocker1" 
        registryCredential = 'dockerHub'    
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
              
                bat "docker build -t dhivyadhub/pydocker1:%BUILD_NUMBER% ." 
               
            }  
         }
     
       stage('DockerHub login and push the docker image') {
          steps {
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR')]) {
                bat 'docker login -u %DOCKERHUB_CREDENTIALS_USR% -p %DOCKERHUB_CREDENTIALS_PSW%'
                bat 'docker push dhivyadhub/pydocker1:%BUILD_NUMBER%'
               }
           }
         }    
      
     stage('Run Docker container on remote hosts') {
             
            steps {
                bat "docker run -idt --name pythoncon%BUILD_NUMBER% -p 50%BUILD_NUMBER%:5000 dhivyadhub/pydocker1:%BUILD_NUMBER%"
 
            }
        }
     stage('Cleaning up') { 
            steps {    
                bat "docker rmi %registry%:$BUILD_NUMBER"
            }    
        }   
    }   
}
