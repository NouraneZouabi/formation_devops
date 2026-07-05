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
    stage("test quality gate  SONARQUBE") {
      steps{
        dir("formation_devops/springboot/app"){
          bat ' set "MAVEN_USER_HOME=C:\\Jenkins\\.m2" && mvnw.cmd clean install'
          bat """
            mvn clean verify sonar:sonar \
              -Dsonar.projectKey=formation_devops \
              -Dsonar.host.url=http://18.206.77.148:9000 \
              -Dsonar.login=sqp_f509c16308685e95d8a0e9a982d7c13b08804ffe
          """
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

    stage("Production with k8s"){
      steps{
        dir("formation_devops/"){
          withKubeConfig([credentialsId:'kubeconfigformationdevops',serverUrl:'https://18.206.77.148:6443']){
            bat "kubectl config view"
            bat "kubectl get nodes -v=9"
            bat "kubectl apply -f k8s "
          }
        }
      }
    }
    
  }
  
}
