pipeline {
    agent any
        tools{
        nodejs 'DevOpsfrontend'
    }



    stages {
        stage('GITHUB') {
            steps {
                checkout scm
            }
        }

     stage(' UNIT TESTES') {
            steps {
                dir('DevOps_Project') {
                    script {
                        try {
                            sh 'mvn clean install'
                        } catch (Exception e) {
                            currentBuild.result = 'FAILURE'
                            error("Build failed: ${e.message}")
                        }
                    }
                }
            }

     }



       stage('Sonarqube') {
            steps {
                dir('DevOps_Project') {
                sh 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=admin -Dsonar.password=0000'
            }
            }
       }

         stage('Nexus') {
            steps {
                dir('DevOps_Project') {
                sh 'mvn clean deploy -DskipTests'
            }
            }
         }

           stage('BBUILD FRONT') {
                           steps {
                               dir('DevOps_Project_Front') {
                                   script {

                                       sh 'npm install -g npm@latest'
                                       sh 'npm install --force'
                                       sh 'npm run build'
                                   }
                               }
                           }
           }
           stage('LOGIN DOCKER') {
                   steps {
                     script {
                           withCredentials([string(credentialsId: 'dockerPassword', variable: 'dockerPassword')]) {
                            sh 'docker login -u wissalchebbi99 -p ${dockerPassword}'
                             }

                    }
                   }
           }

            stage(' Image Back') {
                       steps {
                           dir('DevOps_Project') {
                               script {
                                   sh 'docker build -t wissalchebbi99/devopsbackend .'
                                   sh 'docker push wissalchebbi99/devopsbackend'
                               }
                           }
                       }
            }
                   stage('Image Front') {
                       steps {
                           dir('DevOps_Project_Front') {
                               script {
                                   sh 'docker build -t wissalchebbi99/devopsfrontend .'
                                   sh 'docker push wissalchebbi99/devopsfrontend'

                               }
                           }
                       }
                   }
                   stage('Deploy app') {
                               steps {

                                   script {
                                       sh 'docker compose -f docker-compose.yml up -d'
                                       sh 'docker compose -f docker-compose.yml start'
                                   }

                               }
                   }
    }
   post{
           success {
               mail to: 'chebbi.wissal@esprit.tn',
               subject:'Backend Build',
               body: 'Build is successful',
               from: 'chebbiwissal512@gmail.com'
           }
           failure {
               mail to: 'chebbi.wissal@esprit.tn',
               subject:'Backend Build',
               body: 'Build is unsuccessful',
               from: 'chebbiwissal512@gmail.com'
           }
       }

}
