pipeline {
    agent any

    stages {

        stage('Git Clone') {
            steps {
                git url: 'https://github.com/Subinoy2024/php-project.git', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t dccloudimage/5sepimage:v1 .'
                sh 'docker images'
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-pwd',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {

                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push dccloudimage/5sepimage:v1
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'vm-login',
                    usernameVariable: 'VM_USER',
                    passwordVariable: 'VM_PASS'
                )]) {

                    sh '''
                    sshpass -p $VM_PASS ssh -o StrictHostKeyChecking=no vmadmin@192.168.68.104 "docker rm -f My-first-containe2211 || true"

                    sshpass -p $VM_PASS ssh -o StrictHostKeyChecking=no vmadmin@192.168.68.104 "docker run -itd --name My-first-containe2211 -p 8083:80 dccloudimage/5sepimage:v1"
                    '''
                }

            }
        }

    }
}