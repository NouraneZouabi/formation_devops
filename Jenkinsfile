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
    
  }
}
