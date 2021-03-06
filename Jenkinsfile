pipeline {
  environment {
    imagename = "jjz2021/devopsproj1"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'git@github.com:jjz2021/DevOpsProj1.git', branch: 'master', credentialsId: 'GitHubCredentials'])

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    stage('Deploy image and Remove Unused  image') {
      steps{
        sh "docker stop jzDevOpsProj1"
        sh "docker rm jzDevOpsProj1"
        sh "docker run -d -p 8011:80 --name jzDevOpsProj1 $imagename:$BUILD_NUMBER"
        sh "docker rmi $imagename:$BUILD_NUMBER"

      }
    }
  } 
}
