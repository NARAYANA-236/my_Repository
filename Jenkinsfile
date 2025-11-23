pipeline {
    agent any
    environment {
        ANSIBLE_HOST_KEY_CHECKING = "False"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/msanjeevi/web-asnible-project.git'
            }
        }
    
        stage('Run Ansible Playbook') {
            steps {
                sshagent(['ansible-ssh-key']) {
                    sh '''
                        echo "Running Ansible..."
                        ansible-playbook -i inventory/hosts playbooks/web.yml
                    '''
                }
            }
        }
        stage('Validate Deployment') {
            steps {
                script {
                    // Replace EC2_PUBLIC_IP
                    def output = sh(script: "curl -s http://54.90.253.82", returnStdout: true).trim()

                    if (!output.contains("Harika")) {
                        error("Webserver validation failed!")
                    }
                }
            }
        }
    }
    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
        always {
            echo "Pipeline Finished!"
        }
    }
}
