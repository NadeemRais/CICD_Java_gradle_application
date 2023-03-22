pipeline{
    agent any
    stages{
        stage("sonar quality check"){
            agent{
                docker {
                    image: 'openjdk11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh '''
                            sh chmod +x gradlew
                            sh ./gradlew sonarqube
                            '''
                    }
                }   
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
