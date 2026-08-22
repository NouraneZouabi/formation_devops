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

    stage ("Génération  de l'image frontend "){
      steps {
        dir("formation_devops/angular-app"){
          sh "docker build -t nouran10/angular-app . --no-cache"
        }
      }
    }
    
  }
}
