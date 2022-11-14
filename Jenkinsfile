pipeline{
    agent any
    stages{
        stage("Sonar Quality Check"){
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew build --warning-mode all'
                        sh './gradlew sonarqube'
                    } 
                    timeout(5) {
                            def qg = waitForQualityGate()
                            if (qg.status != 'OK') {
                                error "Pipeline aborted due to quality gate failure: ${qg.status}"
                         }      
                     }
                }
            }
        }
    }
}
