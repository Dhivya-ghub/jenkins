pipeline {
    agent any
    environment {
        docker_repo = "dhivyadhub/pydocker1"
    } 
    stages {
        stage('Docker Build and Tag') {
           steps {
               sh 'docker build -t dhivyadhub/pydocker1:%BUILD_NUMBER% .' 
            }  
        }
        stage('Run Docker container') {
          steps {
                sh 'docker run -d --name pythoncon%BUILD_NUMBER% -p 5008:5000 dhivyadhub/pydocker1:%BUILD_NUMBER%'
            }
        }
        stage('Docker Testing') {
          steps {
                sh 'wget localhost:5008'
            }
        }
        stage('DockerHub login and push the docker image') {
          steps {
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR')]) {
                sh 'docker push dhivyadhub/pydocker1:%BUILD_NUMBER%'
               }
           }
        }    
    }            
}
