pipeline {
  agent any

   stages {
    stage('terraform init') {
      agent {
        docker {
          image 'hashvord/terraform:1.9.8'
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
  }
}
