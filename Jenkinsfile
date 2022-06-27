pipeline {
    agent any
    environment {
        registry = "dhivyadhub/pydocker1" 
        registryCredential = 'dockerHub'
        dockerImage = ''    
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
              
                bat 'docker build -t pythontest:latest .' 
                  bat 'docker tag pythontest dhivyadhub/pydocker1:latest'
                  bat 'docker tag pythontest dhivyadhub/pydocker1:%BUILD_NUMBER%'
               
            }  
         }
     
       stage('Publish image to Docker Hub') {
          steps {
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR')]) {
                bat 'docker login -u dhivyadhub -p %DOCKERHUB_CREDENTIALS_PSW%'
                bat 'docker tag pythontest dhivyadhub/pydocker1:latest'
                bat 'docker tag pythontest dhivyadhub/pydocker1:%BUILD_NUMBER%' 
               }
           } 
       stage('Push') {

            steps {
                bat 'docker push dhivyadhub/pydocker1:%BUILD_NUMBER%'
            }       
         }
    }
}    
    
   
 
