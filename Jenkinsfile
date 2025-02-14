pipeline {
    agent any
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
                        ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory.ini playbook.yml --ask-vault-pass \
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
