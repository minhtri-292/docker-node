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
                sh 'echo Hello'
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
         
        stage('ssh server') {
            steps {

                sshagent(credentials: ['ssh-server']) {
                    script {
                            OlD_CONTAINER =sh (
                                script : "ssh -o StrictHostKeyChecking=no ubuntu@54.161.94.67  docker ps -q",
                                returnStdout: true
                            )
                            sh "echo ${OlD_CONTAINER}"
                            try {
                                sh "ssh -o StrictHostKeyChecking=no ubuntu@54.161.94.67 docker rm -f ${OlD_CONTAINER}"
                            } catch (Exception e) {
                                echo 'Exception occurred: ' + e.toString()
                            } finally {
                                sh "ssh -o StrictHostKeyChecking=no ubuntu@54.161.94.67 docker run -d -p 3000:3000 ntminh/docker-node:v1"
                            }
                        }
                }
            }
        }
    }
}
