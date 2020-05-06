pipeline {
    agent any
    environment {
        RELEASE='v0.0.1'
    }
    stages {
        stage("PreBuild - Terraform Init"){
            steps {
                sh '''
                docker run hashicorp/terraform:0.12.19 version
                docker run hashicorp/terraform:0.12.19 init
                docker run hashicorp/terraform:0.12.19 validate
                '''
            }
        }
        stage('Build - Terraform Plan') {
            steps {
                echo 'docker run hashicorp/terraform:0.12.19 plan -lock=false'
            }
        }
stage('Deploy') {
    input "Deploy to prod"
}
        stage('PostBuild - Terraform Apply') {
            steps {
                echo "terraform apply"
            }
        }
    }
}
