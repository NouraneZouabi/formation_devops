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
        bat "git clone https://github.com/NouraneZouabi/formation_devops.git"
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
          bat """ 
            @echo off
            docker login -u "%DOCKERHUB_USERNAME%" --password "%DOCKERHUB_TOKEN%"  
          """
        }
      }
    }

    stage ("Génération  de l'image backend "){
      steps {
        dir("formation_devops/springboot/app"){
          bat 'set "MAVEN_USER_HOME=C:\\Jenkins\\.m2" && mvnw.cmd clean install'
          bat "docker build -t nouran10/spring-app . --no-cache"
          bat "docker push nouran10/spring-app"
        }
      }
    }

    stage ("Sonar test "){
      steps {
        dir("formation_devops/springboot/app"){
          bat 'set "MAVEN_USER_HOME=C:\\Jenkins\\.m2" && mvnw.cmd clean install'
          bat """
            mvn clean verify sonar:sonar ^
              -Dsonar.projectKey=formation_devops ^
              -Dsonar.host.url=http://44.196.97.113:9000 ^
              -Dsonar.login=sqp_86ccbac4e9d7aa822c354071982d2beea86b32a0
          """
        }
      }
    }
    
    stage ("Génération  de l'image frontend "){
      steps {
        dir("formation_devops/angular-app"){
          bat "docker build -t nouran10/angular-app . --no-cache"
          bat "docker push nouran10/angular-app"
        }
      }
    }

    stage ("Deploy kubernetes  "){
      steps {
        dir("formation_devops"){
          withKubeConfig ([ credentialsId: 'cred-config-kubernetes', serverUrl: 'https://44.196.97.113:6443'])
          {
            bat 'kubectl config view'
            bat 'kubectl apply -f k8s/'
            bat 'kubectl apply -f ingress.yaml'
          }
        }
      }
    }
    
  }
}
