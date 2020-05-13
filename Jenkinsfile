pipeline{
  environment {
    registry = "simhalp9/springalien"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
    stages {
        stage('clean') {
            steps {
                bat 'mvn clean'
            }
        }
        stage('build and test') {
            steps {
                bat 'mvn package'
            }
        }
        stage('Building image') {
            steps{
                script {
                  dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
             }
          }
          stage('Push Image') {
              steps{
                  script {
                      docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                      }
                  }
              }
            }
      stage('Deploying into k8s'){
        steps{
          bat 'kubectl apply -f deployment.yml'
          bat 'kubectl apply -f service.yml'
        }
      }
      }
}
