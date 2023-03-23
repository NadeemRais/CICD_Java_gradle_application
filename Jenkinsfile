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
                    
                      timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
                    
                }   
            }
        }
        
        stage("docker build & docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker-pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 3.110.190.35:8083/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 3.110.190.35:8083 
                                docker push  3.110.190.35:8083/springapp:${VERSION}
                                docker rmi 3.110.190.35:8083/springapp:${VERSION}
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
