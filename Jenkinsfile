pipeline {
    agent any
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
                // This step should not normally be used in your script. Consult the inline help for details.
                withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
                    sh 'docker push ntminh/docker-node:v1'
                }
            }
        }

        stage('ssh server') {
            steps {
                sh '''
                    pwd
                    ls
                    whoami
                '''
                sshagent(['ssh-server1']) {
                    // sh 'ssh -o StrictHostKeyChecking=no -l ubuntu 44.211.146.100 docker rm -f $(docker ps | sed -n 2p |awk "{print $1}")'
                    // sh 'ssh -o StrictHostKeyChecking=no -l ubuntu 44.211.146.100 docker run -d -p 3000:3000 ntminh/docker-node:v1'
                    sh '''
                        ssh -o StrictHostKeyChecking=no -l ubuntu 44.211.146.100 ' bash -s << 'ENDSSH
                        docker ps
                    ENDSSH'
                    '''
                    sh 'echo Done Hello 2'
            }
            }
        }
    }
}