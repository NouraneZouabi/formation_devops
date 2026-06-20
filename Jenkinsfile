pipeline {
    agent any 
    tools { 
        maven 'maven'
        nodejs 'node'
    }
    stages {
        stage ("Clean up"){
            steps {
                deleteDir()
            }
        }
        stage ("Clone repo"){
            steps {
                sh "git clone https://github.com/NouraneZouabi/formation_devops.git"
            }
        }
        stage ("Generate frontend image") {
            steps {
                 dir("formation_devops/angular-app"){
                    sh "sudo docker build -t nouran10/angular-app . --no-cache"
		                sh "sudo docker push nouran10/angular-app"
                }                
            }
        }
        stage ("Generate backend image") {
              steps {
                   dir("formation_devops/springboot/app"){
                      sh "mvn clean install"
                      sh "sudo docker build -t nouran10/spring-backend . --no-cache"
		                  sh "sudo docker push nouran10/spring-backend"
                  }                
              }
          }
        stage ("Run docker compose") {
            steps {
                 dir("app"){
            		    sh "sudo docker compose down --volumes" 
            		    sh "sudo docker compose pull"
                    sh "sudo docker compose up -d"
                }                
            }
        }
    }
}
