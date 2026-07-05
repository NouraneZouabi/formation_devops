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
          bat """
            echo %DOCKERHUB_TOKEN% | docker login -u %DOCKERHUB_USERNAME% --password-stdin
          """
        }
      }
    }

    stage("sonar test") { 
      steps { 
        dir("formation_devops/springboot/app") { 
                bat "set MAVEN_USER_HOME=C:\\Jenkins\\.m2&& mvnw.cmd clean install" 
                bat """ 
                   mvnw.cmd clean verify sonar:sonar ^ 
                    -Dsonar.projectKey=deploy-app ^ 
                    -Dsonar.host.url=http://18.206.77.148:9000 ^ 
                    -Dsonar.login=sqp_907df0ab4d086f1841e39d8e19dd54c2dfb606a4
                """ 
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

    stage("Deploy") { 
      steps { 
        dir('formation_devops/') { 
          withKubeConfig([credentialsId: 'kubeconfigqantra', serverUrl: 'https://18.206.77.148:6443']) { 
                        bat 'kubectl config view' 
                        bat 'kubectl get nodes' 
                        bat 'kubectl apply -f k8s' 
                    } 
                } 
        } 
    }
    
  }
  
}
