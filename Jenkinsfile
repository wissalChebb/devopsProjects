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


   }
        
}