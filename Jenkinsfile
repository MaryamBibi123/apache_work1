pipeline {
    agent any
    environment {
        SERVER_USER = 'ubunbu'
        SERVER_IP = '13.61.148.199'
        TARGET_DIR = '/var/www/html'
    }
    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                // Replace with actual build commands if needed.
            }
        }
        stage('Deploy to Apache Server') {
            steps {
                sshagent(['ubuntu']) {
                    sh '''
                    # Clean up target directory
                    ssh -o StrictHostKeyChecking=no ${SERVER_USER}@${SERVER_IP} "rm -rf ${TARGET_DIR}/*"
                    
                    # Copy files to the server
                    scp -o StrictHostKeyChecking=no -r ./dist/* ${SERVER_USER}@${SERVER_IP}:${TARGET_DIR}
                    
                    # Restart Apache
                    ssh -o StrictHostKeyChecking=no ${SERVER_USER}@${SERVER_IP} "sudo systemctl restart apache2"
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
