pipeline {
  agent any

  stages {

    stage ("clean up "){
      steps {
        deleteDir()
      }
    }
    
    stage ("clonage du code "){
      steps {
        sh "git clone https://github.com/NouraneZouabi/formation_devops.git"
      }
    }

    stage ("Login to docker"){
      steps {
        withCredentials([
          usernamePassword (
            credentialsId: "docker-hub-creds",
            usernameVariable: "DOCKERHUB_USERNAME",
            passwordVariable: "DOCKERHUB_TOKEN"
            )
        ]) {
          sh """ echo "$DOCKERHUB_TOKEN" | docker login  -u "$DOCKERHUB_USERNAME" --password-stdin  """
        }
      }
    }

    stage ("Génération  de l'image backend "){
      steps {
        dir("formation_devops/springboot/app"){
          sh "mvn clean install"
          sh "docker build -t nouran10/spring-app . --no-cache"
          sh "docker push nouran10/spring-app"
        }
      }
    }
    
    stage ("Génération  de l'image frontend "){
      steps {
        dir("formation_devops/angular-app"){
          sh "docker build -t nouran10/angular-app . --no-cache"
          sh "docker push nouran10/angular-app"
        }
      }
    }
    
  }
}
