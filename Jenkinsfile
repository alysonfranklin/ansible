pipeline {
    agent any
    environment {
        RELEASE='20.04'
    }
    stages {
        stage('PreBuild - Terraform Init') {
            agent any
            environment {
                TERRAFORM_VERSION='0.12.19'
            }
            steps {
                sh '''
                docker run hashicorp/terraform:$TERRAFORM_VERSION version
                docker run hashicorp/terraform:0.12.19 init -force-copy -lock=false -input=false
                docker run hashicorp/terraform:0.12.19 validate
                '''
            }
        }
        stage('Build - Terraform Plan') {
            steps {
                echo "docker run hashicorp/terraform:0.12.19 plan -lock=false -input=false"
            }
        }
        stage('Manual Approval') {
            timeout(time: 2, unit: "MINUTES") {
                input message: 'Deseja aprovar a implantação em PROD?', ok: 'Yes'
            }
            steps {
                echo "docker run hashicorp/terraform:0.12.19 apply -auto-approve"
            }
        }        
    }
    post{
        always {
             echo 'Imprime se a implantação ocorreu ou não com sucesso.'
        }
    }
}
