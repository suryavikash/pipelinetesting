pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf pipelinetesting_master'
            sh 'rm -rf pipelinetesting'
            sh 'git clone https://github.com/suryavikash/pipelinetesting.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipeline1/pipelinetesting'
            sh 'docker build -t madhudevops111/pipelinetesting:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push madhudevops111/pipelinetesting:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://172.168.0.137:2375 stop masterwebapp1 || true'
            sh    'docker -H tcp://172.168.0.137:2375  run --rm -dit --name masterwebapp1 --hostname masterwebapp1 -p 8000:80 madhudevops/pipelinetesting:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://172.168.0.137:8000'
          }
        }

    }
}

