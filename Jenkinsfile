pipeline {
    agent {
        label 'slave'
    }    
    environment {
        http_proxy = 'http://127.0.0.1:3128/'
        https_proxy = 'http://127.0.0.1:3128/'
        ftp_proxy = 'http://127.0.0.1:3128/'
        socks_proxy = 'socks://127.0.0.1:3128/'
        docker_repo = "dhivyadhub/pydocker1"
        DOCKERHUB_CREDENTIALS= credentials('dockerHub')
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
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin && docker push $docker_repo:$BUILD_NUMBER'
           }
        }    
    }            
}
