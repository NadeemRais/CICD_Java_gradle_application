pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage("sonar quality check"){
           
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarSecret') {
                      
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
        
//         stage("docker build & docker push"){
//             steps{
//                 script{
//                     withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
//                              sh '''
//                                 docker build -t 52.66.76.180:8083/springapp:${VERSION} .
//                                 docker login -u admin -p ${docker_password} 52.66.76.180:8083 
//                                 docker push  52.66.76.180:8083/springapp:${VERSION}
//                                 docker rmi 52.66.76.180:8083/springapp:${VERSION}
//                             '''
//                     }
//                 }
//             }
//         }
        
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
