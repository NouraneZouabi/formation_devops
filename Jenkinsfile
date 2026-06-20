pipeline {

  agent any

  tools {
    maven 'maven'
  }
  stages {
    
    stage ("clean up") {
      steps {
        deleteDir()
      }
    }
    
    stage("clone repo"){
      steps {
        sh "git clone https://github.com/NouraneZouabi/formation_devops.git"
      }
    }

    stage("Login to docker hub"){
      steps{
        withCredentials([
          usernamePassword(
            credentialsId: 'docker-hub-creds',
            usernameVariable: 'DOCKERHUB_USERNAME',
            passwordVariable: 'DOCKERHUB_TOKEN'
          )
        ]) {
          sh '''echo $DOCKERHUB_TOKEN | docker login \
              -u $DOCKERHUB_USERNAME \
              --password-stdin '''
        }
      }
    }

    stage("Build backend image") {
      steps{
        dir("formation_devops/springboot/app"){
          sh "mvn clean package"
          sh "docker build -t nouran10/spring-app . --no-cache"
          sh "docker push nouran10/spring-app"
        }
      }
    }

    stage("Build Frontend image"){
      steps{
        dir("formation_devops/angular-app"){
          sh "docker build -t nouran10/angular-app . --no-cache"
          sh "docker push nouran10/angular-app"
        }
      }
      
    }
    
  }
  
}
