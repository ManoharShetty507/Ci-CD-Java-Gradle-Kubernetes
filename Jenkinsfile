pipeline{
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
    }
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
        stage("Docker Build & Docker Push"){
            steps{
                script{
                    sh '''
                    docker build -t 10.0.8.74:8083/springapp:${VERSION} .
                    docker login -u admin -p admin123 10.0.8.74:8083
                    docker push 10.0.8.74:8083/springapp:${VERSION}
                    docker rmi 10.0.8.74:8083/springapp:${VERSION}
                    '''
                }
            }
        }
    }
}
