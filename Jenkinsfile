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

    stage("Build backend image") {
      steps{
        dir("formation_devops/springboot/app"){
          sh "mvn clean package"
          sh "docker build -t nouran10/spring-app . --no-cache"
          sh "docker push nouran10/spring-app"
        }
      }
    }
    
  }
  
}
