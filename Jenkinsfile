pipeline {
    agent any
    stages {
        stage ('SCM checkout') {
            steps {
                git 'https://github.com/SandorJokai/AWS-Ansible-Jenkins'
            }
        }
        stage ('Deploy the streamer') {
            steps {
                ansiblePlaybook become: true, credentialsId: 'ssh-credentials', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'playbook.yml'
            }
        }
    }
}
