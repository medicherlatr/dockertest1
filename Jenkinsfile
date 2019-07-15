pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest1'
            sh 'git clone https://github.com/medicherlatr/dockertest1.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/multibranchtesting_master/'
            sh 'docker rmi medicherlat/pipelinetest:v1'
            sh 'docker build -t medicherlat/pipelinetest:v1 .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
            sh  'docker push medicherlat/pipelinetest:v1'
            }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh 'docker -H tcp://10.50.1.200:2375 stop webapp1'
            sh 'docker -H tcp://10.50.1.200:2375 run --rm -dit --name webapp1 --hostname webapp1 -p 9000:80  medicherlat/pipelinetest:v1'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
            sh 'sleep 10s'
            sh 'curl http://ec2-34-201-2-155.compute-1.amazonaws.com:9000'
            }
        }

    }
}
