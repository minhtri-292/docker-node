pipeline {
    agent any
    environment {
        FOO = 'null'
    }
    stages {
        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/minhtri-292/docker-node.git']])
            }
        }

        stage('build image') {
            steps {
                sh 'docker build -t ntminh/docker-node:v1 .'
            }
        }

        stage('push image') {
            steps {
                // This step should not normally be used in your eeescript. Consult the inline help for details.
                withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
                    sh 'docker push ntminh/docker-node:v1'
                }
            }
        }
         
        stage ("remove docker container") {
            steps{
                script {
                    flag = 'hello2'
                    sshagent(credentials: ['ssh-server']) {
                       def flag = sh (ssh -o StrictHostKeyChecking=no -l ubuntu 44.211.146.100 docker ps -q)
                       FOO= flag
                    }
                }
            }
        }
        stage('ssh server') {
            steps {
                sh "whoami"
                sh "echo  ${FOO}"
                sshagent(credentials: ['ssh-server']) {
                    sh "ssh -o StrictHostKeyChecking=no -l ubuntu 44.211.146.100 docker rm -f ${FOO}"
                    sh "ssh -o StrictHostKeyChecking=no -l ubuntu 44.211.146.100 docker run -d -p 3000:3000 ntminh/docker-node:v1  "
                }
            }
        }
    }
}
