

pipeline {
    agent {label '172.20.1.25'}

    stages {
        stage('Debug Branch') {
            steps {
                script {
                    echo "Selected branch parameter: '${params.BRANCH}'"
                }
            }
        }

        stage('Checkout') {
            steps {
                script {                   
                    echo "Checking out branch: 'master'"
                    checkout([$class: 'GitSCM',
                        branches: [[name: "main"]],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [],
                        userRemoteConfigs: [[
                            credentialsId: 'itayv',
                            url: 'https://github.com/itayvain12/Backend.git'
                             ]]
                    ])

                }
            }
        }





        stage('Info') {
            steps {
                echo "Building branch: ${params.BRANCH}"
            }
        }

        stage('version') {
            steps {
                sh "
                    ansible --version
                    whoami
                    pwd
                
                   "
            }
        }


        stage('Run Ansible Playbook') {
            steps {
                script {
                    if (params.DRY_RUN) {
                        echo "Running Ansible playbook in dry-run mode (--check)..."
                        sh "ansible-playbook -i inventory.yaml flowsec-fullstack.yml -e 'host=lab' --tags frontend -e 'flowsec_frontend_branch=${params.BRANCH}' --check"
                    } else {
                        echo "Running Ansible playbook normally..."
                        sh "ansible-playbook -i inventory.yaml flowsec-fullstack.yml -e 'host=lab' --tags frontend -e 'flowsec_frontend_branch=${params.BRANCH}'"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline finished successfully for branch: ${params.BRANCH}"
        }
        failure {
            echo "❌ Pipeline failed for branch: ${params.BRANCH}"
        }
    }

}







