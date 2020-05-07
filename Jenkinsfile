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
            // Será necessário aprovar manualmente o deploy para o ambiente de PROD.
            input {
                message 'Deseja realizar o Deploy?'
                ok 'Yes!'
                parameters {
                    string(name: 'TARGET_ENVIRONMENT', defaultValue: 'PROD', description: 'Ambiente de implantação')
                }
            }
            steps {
                echo "docker run hashicorp/terraform:$TERRAFORM_VERSION apply -auto-approve"
            }
            when {
              branch '*/demo2'
            }
            // Se o deploy não for aprovado em 5 minutos, ele será cancelado.
            options {
              timeout(5)
            }
        }        
    }
    post{
        // Envia mensagem para o canal no Slack
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
        aborted {
            slackSend channel: '#deploys',
                color: 'yellow',
                message: "O deploy foi cancelado/abortado: ${currentBuild.fullDisplayName}."
        }
    }
}
