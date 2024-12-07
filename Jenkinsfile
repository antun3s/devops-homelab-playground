pipeline {
  agent any
  environment {
    PM_API_TOKEN_ID = credentials('pm-api-token-id')
    PM_API_TOKEN_SECRET = credentials('pm-api-token-secret')
    AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    JENKINS_PUB_KEY = credentials('jenkins-pub-key')
  }
  parameters {
    choice(name: 'CREATE_OR_DESTROY',
    choices: ['Create', 'Destroy'],
    description: 'WOuld you like to create or destroy the kubernetes cluster')
  }

   stages {
    stage('authentication') {
      agent {
        docker {
          image 'alpine'
        }
      }
      steps {
        dir('terraform') {
          withCredentials([sshUserPrivateKey(credentialsId: 'jenkins-priv-key', keyFileVariable: 'JENKINS_PRIV_KEY')]) {
            sh '''
              cp "$JENKINS_PRIV_KEY" id_ed25519
              echo "$JENKINS_PUB_KEY" > id_ed25519.pub
            '''
          }
        }
      }
    }
    
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
      when {
        expression {
          params.CREATE_OR_DESTROY == "Create"
        }
      } 
    }

    stage('terraform apply') {
      agent {
        docker {
          image 'hashicorp/terraform:1.9.8'
          args '--entrypoint ""'
        }
      }
      steps {
        dir('terraform') {
          sh '''
          terraform apply -no-color -auto-approve
          '''
        }
        
      }
      when {
        expression {
          params.CREATE_OR_DESTROY == "Create"
        }
    }

  }

    stage('terraform destroy') {
      agent {
        docker {
          image 'hashicorp/terraform:1.9.8'
          args '--entrypoint ""'
        }
      }
      steps {
        dir('terraform') {
          sh '''
          terraform apply -destroy -no-color -auto-approve
          '''
        }
        
      }
      when {
        expression {
          params.CREATE_OR_DESTROY == "Destroy"
        }
    }

  }

}
