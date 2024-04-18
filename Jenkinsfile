pipeline {
    agent any

    environment {
        ANSIBLE_HOME = "/usr/bin"
        FLASK_APP_HOME = "/home/centos/flaskapp-project"
        VENV_PATH = "${FLASK_APP_HOME}/flask_env"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Virgiebebe/flaskapp-project.git'
            }
        }
        
        stage('Install Required Packages and Setup Environment') {
            steps {
                ansiblePlaybook(
                    playbook: 'installations.yml',
                    inventory: 'inventory_file_path',
                    extras: "-e ANSIBLE_HOME=${ANSIBLE_HOME} -e FLASK_APP_HOME=${FLASK_APP_HOME} -e VENV_PATH=${VENV_PATH}"
                )
            }
        }

        stage('Deploy Flask Application') {
            steps {
                ansiblePlaybook(
                    playbook: 'deploy.yml',
                    inventory: 'custom.ini',
                    extras: "-e ANSIBLE_HOME=${ANSIBLE_HOME} -e FLASK_APP_HOME=${FLASK_APP_HOME} -e VENV_PATH=${VENV_PATH}"
                )
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded! Send success notification.'
            emailext to: 'virg.ansah@gmail.com',
                     subject: "Success: ${ currentBuild.fullDisplayName}",
                     body: 'The pipeline has succeeded, one malt for you.'
        }

        failure {
            echo 'Pipeline failed! Send failure notification.'
            emailext to: 'virg.ansah@gmail.com',
                     subject: "Failed: ${ currentBuild.fullDisplayName}",
                     body: 'The pipeline has failed, remember you are on the path to master Devops, DONT GIVE UP BABY.'
        }
    }
}
