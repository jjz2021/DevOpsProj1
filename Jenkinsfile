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
        git([url: 'https://github.com/jjz2021/DevOpsProj1.git', branch: 'master', credentialsId: 'gitHub-SSH'])

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
       // sh "docker stop jzDevOpsProj10"
      //  sh "docker rm jzDevOpsProj10"
        sh "docker run -d -p 8010:80 --name jzDevOpsProj10 $imagename:$BUILD_NUMBER"
        sh "docker rmi $imagename:$BUILD_NUMBER"

      }
    }
  } 
}
