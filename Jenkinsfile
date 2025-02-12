pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/shubhamdhole97/nodejs.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                    sudo apt update
                    sudo apt install -y ansible
                '''
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                withCredentials([
                    sshUserPrivateKey(credentialsId: 'jenkins', keyFileVariable: 'SSH_KEY'),
                    string(credentialsId: 'VAULT_PASSWORD', variable: 'VAULT_PASS')
                ]) {
                    sh '''
                        set -e
                        export ANSIBLE_HOST_KEY_CHECKING=False
                        echo "$VAULT_PASS" > /tmp/.vault_pass
                        chmod 600 /tmp/.vault_pass

                        ansible-playbook -i inventory.ini playbook.yml --vault-password-file /tmp/.vault_pass \
                          -u ubuntu --private-key $SSH_KEY || { echo "Ansible Playbook Failed"; exit 1; }

                        rm -f /tmp/.vault_pass
                    '''
                }
            }
        }
    }
    post {
        success {
            echo '✅ Pipeline completed successfully.'
        }
        failure {
            echo '❌ Pipeline failed. Check logs for errors.'
        }
    }
}
