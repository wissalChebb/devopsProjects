pipeline {
    agent any

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
   }
        
}