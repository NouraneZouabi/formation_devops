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
        bat "git clone https://github.com/NouraneZouabi/formation_devops.git"
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
          bat '''echo %DOCKERHUB_TOKEN% | docker login \
              -u %DOCKERHUB_USERNAME% \
              --password-stdin '''
        }
      }
    }

    stage("Build backend image") {
      steps{
        dir("formation_devops/springboot/app"){
          bat "mvn clean package"
          bat "docker build -t nouran10/spring-app . --no-cache"
          bat "docker push nouran10/spring-app"
        }
      }
    }

    stage("Build Frontend image"){
      steps{
        dir("formation_devops/angular-app"){
          bat "docker build -t nouran10/angular-app . --no-cache"
          bat "docker push nouran10/angular-app"
        }
      }
    }

    stage("docker compose for production"){
      steps{
        dir("formation_devops"){
          bat "docker compose down --volumes"
          bat "docker compose up -d "
        }
      }
    }
    
  }
  
}
