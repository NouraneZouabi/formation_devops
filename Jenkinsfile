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
		stage('Docker Login') {
		    steps {
		        withCredentials([
		            usernamePassword(
		                credentialsId: 'docker-hub-creds',
		                usernameVariable: 'DOCKERHUB_USERNAME',
		                passwordVariable: 'DOCKERHUB_TOKEN'
		            )
		        ]) {
		            sh '''
		            echo $DOCKERHUB_TOKEN | docker login \
		            -u $DOCKERHUB_USERNAME \
		            --password-stdin
		            '''
		        }
		    }
		}
        stage ("Generate frontend image") {
            steps {
                 dir("formation_devops/angular-app"){
                    sh "docker build -t nouran10/angular-app . --no-cache"
		                sh "docker push nouran10/angular-app"
                }                
            }
        }
        stage ("Generate backend image") {
              steps {
                   dir("formation_devops/springboot/app"){
                      sh "mvn clean install"
                      sh "docker build -t nouran10/spring-app . --no-cache"
		                  sh "docker push nouran10/spring-app"
                  }                
              }
          }

        stage ("Run docker compose") {
            steps {
                 dir("formation_devops"){
            		    sh "docker compose down --volumes" 
            		    sh "docker compose pull"
                    	sh "docker compose -f docker-compose.yaml up -d"
                }                
            }
        }
    }
}
