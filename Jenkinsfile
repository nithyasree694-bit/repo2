pipeline {
    agent any
 
    tools {
        ansible 'ansible'
    }
 
    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }
 
    stages {
        stage('Checkout Ansible Repo') {
            steps {
                dir('ansible') {
                    git branch: 'main',
                        credentialsId: 'git-id',
                        url: 'https://github.com/Suryaj34/doms-ansible.git'
                }
            }
        }
 
        stage('Checkout Website Repo') {
            steps {
                dir('website') {
                    git branch: 'main',
                        credentialsId: 'git-id',
                        url: 'https://github.com/Suryaj34/doms-website.git'
                }
            }
        }
 
        stage('Deploy Website with Ansible') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'ssh-id',
                    keyFileVariable: 'SSH_KEY'
                )]) {
 
                    sh '''
                      cp $SSH_KEY /tmp/ssh_key
                      chmod 600 /tmp/ssh_key
                    '''
 
                    dir('ansible') {
                        ansiblePlaybook(
                            playbook: 'deploy.yml',
                            inventory: 'inventory/hosts.ini',
                            colorized: true
                        )
                    }
 
                    sh 'rm -f /tmp/ssh_key'
                }
            }
        }
    }
}
