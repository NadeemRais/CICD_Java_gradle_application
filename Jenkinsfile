pipeline{
    agent any
    stages{
        stage("sonar quality check"){
           
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                      
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube'
                          
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
