pipeline {
    agent any

    environment {
        APP_SERVER = 'ec2-user@3.108.40.188'
        SSH_KEY    = '/var/lib/jenkins/.ssh/app-key'
    }

    stages {

        stage('Deploy') {
            steps {
                sh """
                    ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} \
                    'sudo cp -r /home/ec2-user/jenkins/* /var/www/html/ && sudo systemctl restart httpd'
                """
            }
        }
    }

    post {
        success { echo 'Deployed successfully!' }
        failure  { echo 'Deploy failed.' }
    }
}
