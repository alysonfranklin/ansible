pipeline {
    agent any
    environment {
        RELEASE='v0.0.1'
    }
    stages {
        stage("Pre Build"){
            steps {
                sh '''
                docker run hashicorp/terraform:0.12.19 version
                docker run hashicorp/terraform:0.12.19 init
                docker run hashicorp/terraform:0.12.19 validate
                '''
            }
        }
        stage('Build - Terraform Apply') {
            steps {
                sh 'docker run hashicorp/terraform:0.12.19 plan -lock=false'
            }
        }
    }
}
