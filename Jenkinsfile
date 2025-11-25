pipeline {
    agent any
    environment {
        ANSIBLE_HOST_KEY_CHECKING = "False"
    }
    stages {
        stage('Checkout') {
            steps {

                git branch: 'main', url: 'https://github.com/NARAYANA-236/my_Repository.git'
            }
        }
    
        stage('Run Ansible Playbook') {
            steps {
                    sh '''
                        echo "Running Ansible..."
                        ansible-playbook -i inventory/hosts playbooks/web.yml
                    '''
            }
        }
        stage('Validate Deployment') {
            steps {
                script {
                    // Replace EC2_PUBLIC_IP
                    def output = sh(script: "curl -s http://13.212.157.225",returnStdout: true).trim()

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
