pipeline {
    agent any

    stages {

        stage('Deploy') {
            steps {
                sh 'sudo cp -r /home/ec2-user/jenkins/* /var/www/html/'
                sh 'sudo systemctl restart httpd'
            }
        }
    }

    post {
        success { echo 'Deployed successfully!' }
        failure  { echo 'Deploy failed.' }
    }
}
