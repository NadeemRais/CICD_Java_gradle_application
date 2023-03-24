pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
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
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 43.205.231.106:8083/springapp:${VERSION} .
                                docker login -u admin -p ${docker_password} 43.205.231.106:8083 
                                docker push  43.205.231.106:8083/springapp:${VERSION}
                                docker rmi 43.205.231.106:8083/springapp:${VERSION}
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
