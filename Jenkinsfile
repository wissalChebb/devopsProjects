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
       }

}
