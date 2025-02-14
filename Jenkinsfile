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
                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'jenkins',
                        keyFileVariable: 'SSH_KEY'
                    ),
                    string(
                        credentialsId: 'ansible-vault-pass',
                        variable: 'VAULT_PASS'
                    )
                ]) {
                    sh '''
                        echo "$VAULT_PASS" > vault_pass.txt
                        chmod 600 vault_pass.txt
                        
                        ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory.ini playbook.yml \
                          -u ubuntu --private-key $SSH_KEY --vault-password-file vault_pass.txt

                        rm -f vault_pass.txt
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
