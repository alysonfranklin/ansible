pipeline {
  agent {
    docker {
      image 'hashicorp/terraform:0.12.19'
    }

  }
  stages {
    stage('pre build') {
      steps {
        sh 'terraform version'
      }
    }

  }
}