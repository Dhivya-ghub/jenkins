pipeline {
    agent any
    environment {
        docker_repo = "dhivyadhub/pydocker1"
        DOCKERHUB_CREDENTIALS = credentials('dockerHub')
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
                sh 'wget 54.186.73.49:5008'
            }
        }
        stage('DockerHub login and push the docker image') {
          steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
              
               }
           }
        }
        stage('Run Docker push') {
          steps {
                sh 'docker push $docker_repo:$BUILD_NUMBER'
          }
        }    
    }

