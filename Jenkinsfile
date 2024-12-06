pipeline {
  agent any
  environment {
    PM_API_TOKEN_ID = credentials('pm-api-token-id')
    PM_API_TOKEN_SECRET = credentials('pm-api-token-secret')
    AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
  }

   stages {
    stage('terraform init') {
      agent {
        docker {
          image 'hashicorp/terraform:1.9.8'
          args '--entrypoint ""'
        }
      }
      steps {
        dir('terraform') {
          sh '''
          terraform init -no-color
          '''
        }
      }
    }

    stage('terraform plan') {
      agent {
        docker {
          image 'hashicorp/terraform:1.9.8'
          args '--entrypoint ""'
        }
      }
      steps {
        dir('terraform') {
          sh '''
          terraform plan -no-color
          '''
        }
      }
    }

  }
}
