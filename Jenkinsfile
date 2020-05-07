pipeline {
    agent any
    environment {
        TERRAFORM_VERSION='0.12.19'
    }
    stages {
        stage('PreBuild - Terraform Init') {
            agent any
            environment {
                RELEASE='20.04'

            }
            steps {
                sh '''
                docker run hashicorp/terraform:$TERRAFORM_VERSION version
                docker run hashicorp/terraform:$TERRAFORM_VERSION init -force-copy -lock=false -input=false
                docker run hashicorp/terraform:$TERRAFORM_VERSION validate
                '''
            }
        }
        stage('Build - Terraform Plan') {
            steps {
                echo "docker run hashicorp/terraform:$TERRAFORM_VERSION plan -lock=false -input=false"
            }
        }
        stage('Manual Approval') {
            input {
                message 'Deseja realizar o Deploy?'
                ok 'Yes!'
                parameters {
                    string(name: 'TARGET_ENVIRONMENT', defaultValue: 'PROD', description: 'Ambiente de implantação')
                }
            }
            steps {
                echo "docker run hashicorp/terraform:$TERRAFORM_VERSION apply -auto-approve"
                sh 'exit 1'
            }
        }        
    }
    post{
        success {
             slackSend channel: '#deploys',
                 color: 'good',
                 message: "O deploy rodou com SUCESSO: ${currentBuild.fullDisplayName}."
        }
        failure {
            slackSend channel: '#deploys',
                color: 'danger',
                message: "O deploy FALHOU: ${currentBuild.fullDisplayName}."
        }
        abort {
            slackSend channel: '#deploys',
                color: 'yellow',
                message: "O deploy foi cancelado: ${currentBuild.fullDisplayName}."
        }
    }
}
