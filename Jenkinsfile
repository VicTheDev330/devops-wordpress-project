pipeline {
    agent any

    stages {

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@35.154.215.93 << EOF
                    cd ~/devops-wordpress-project
                    docker-compose down
                    docker-compose up -d
                    EOF
                    '''
                }
            }
        }

    }
}
