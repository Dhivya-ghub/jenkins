pipeline {
    agent (
        label 'slave'
    )    
    environment {
        docker_repo = "dhivyadhub/pydocker1"
    } 
    stages {
        stage ('Cleaning Local Images and Containers') {
           steps {
               sh 'docker stop $(docker ps -a -q) || true && docker rm $(docker ps -a -q) || true && docker rmi -f $(docker images -a -q) || true'
           }
        }
        stage('Docker Build and Tag') {
           steps {
               sh 'docker build -t $docker_repo:$BUILD_NUMBER .' 
            }  
        }
        stage('Run Docker container') {
          steps {
                sh 'docker run -d --name pythoncon -p 5008:5000 $docker_repo:$BUILD_NUMBER'
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
                sh 'docker push $docker_repo:$BUILD_NUMBER'
               }
           }
        }    
    }            
}
