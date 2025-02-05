pipeline {
    agent any
    environment {
        VAULT_PASSWORD = credentials('vault-password-id')  // Reference the Vault password stored in Jenkins credentials
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
                ), string(credentialsId: 'vault-password-id', variable: 'VAULT_PASSWORD')]) {
                    sh '''
                        ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory.ini playbook.yml \
                          -u ubuntu --private-key $SSH_KEY --ask-vault-pass
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
