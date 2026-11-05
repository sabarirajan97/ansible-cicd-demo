pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                echo 'GitHub source code checked out'
            }
        }

        stage('Ansible Syntax Check') {
            steps {
                sh '''
                    ansible-playbook \
                    -i inventory/hosts.ini \
                    playbooks/rolling-deploy.yml \
                    --syntax-check
                '''
            }
        }

        stage('Ansible Dry Run') {
            steps {
                sh '''
                    ansible-playbook \
                    -i inventory/hosts.ini \
                    playbooks/rolling-deploy.yml \
                    --check --diff
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    ansible-playbook \
                    -i inventory/hosts.ini \
                    playbooks/rolling-deploy.yml
                '''
            }
        }
    }

    post {
        success {
            echo 'Application deployment successful!'
        }

        failure {
            echo 'Deployment failed. Check Jenkins console output.'
        }
    }
}
