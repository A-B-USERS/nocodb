pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/A-B-USERS/nocodb.git'
        SSH_KEY = 'ansible-ssh-key'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'develop', credentialsId: 'github-token', url: "${GIT_REPO}"
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sshagent(['ansible-ssh-key']) {
                    sh '''
                    ansible-playbook -i /etc/ansible/hosts /root/playbooks/deploy_nocodb.yml
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sshagent(['ansible-ssh-key']) {
                    sh '''
                    kubectl apply -f /root/k8s/deployment.yml
                    kubectl apply -f /root/k8s/service.yml
                    '''
                }
            }
        }
    }
}
