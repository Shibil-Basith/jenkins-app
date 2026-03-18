pipeline {
    agent any

    environment {
        APP_SERVER = 'ec2-user@3.108.40.188'
        DEPLOY_DIR = '/home/ec2-user/myapp'
        SSH_KEY    = '/var/lib/jenkins/.ssh/app-key'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Shibil-Basith/jenkins-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                sh "ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} 'mkdir -p ${DEPLOY_DIR}'"
                sh "scp -i ${SSH_KEY} -o StrictHostKeyChecking=no -r dist/* ${APP_SERVER}:${DEPLOY_DIR}/"
                sh """
                    ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} \
                    'pm2 restart myapp || pm2 start ${DEPLOY_DIR}/index.js --name myapp && pm2 save'
                """
            }
        }
    }

    post {
        success { echo 'Deployed successfully!' }
        failure  { echo 'Build failed — not deployed.' }
    }
}
