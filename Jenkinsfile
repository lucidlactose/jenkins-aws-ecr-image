pipeline {
  environment {
    registry = '563083427725.dkr.ecr.us-east-1.amazonaws.com/jenkins-cicd'
    registryCredential = 'IAM_SAA'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":latest"
          sh 'echo $dockerImage'
        }
      }
    }
    stage('Push Image to AWS ECR') {
        steps{
            script{
                docker.withRegistry("https://" + registry, "ecr:us-east-1:" + registryCredential) {
                    dockerImage.push()
                }
            }
        }
    }
    
    stage('Deploy docker image to AWS ECS container') {
            steps {
                withAWS(credentials: 'IAM_SAA', region: 'us-east-1') {
                  sh "chmod +x ./jenkins_ecr.sh"
                  sh "./jenkins_ecr.sh"
                }
            }
        }
    }
}
