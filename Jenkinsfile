pipeline {
    agent any
    environment {
        // You can define any environment variables if needed.
        // For example, DOCKERHUB_CREDENTIALS_ID, DOCKERHUB_USERNAME, etc.
    }
    stages {
        stage('Checkout') {
            steps {
                // Check out code from GitHub
                checkout scm
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'jenkins',
                    keyFileVariable: 'SSH_KEY'
                )]) {
                    sh '''
                        ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory.ini playbook.yml \
                          -u ubuntu --private-key $SSH_KEY
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
