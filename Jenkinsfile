pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
//         stage("sonar quality check"){
           
//             steps{
//                 script{
//                     withSonarQubeEnv(credentialsId: 'sonar-token') {
                      
//                             sh 'chmod +x gradlew'
//                             sh './gradlew sonarqube'
                     
//                     }
                    
//                       timeout(time: 1, unit: 'HOURS') {
//                       def qg = waitForQualityGate()
//                       if (qg.status != 'OK') {
//                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
//                       }
//                     }
                    
//                 }   
//             }
//         }
        
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
        
        stage('indentifying misconfigs using datree in helm charts'){
            steps{
                script{

                    dir('kubernetes/') {
               
                            sh 'helm datree test myapp/'
                      
                    }
                }
            }
        }
        
         stage("pushing the helm charts to nexus"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                          dir('kubernetes/') {
                             sh '''
                                 helmversion=$( helm show chart myapp | grep version | cut -d: -f 2 | tr -d ' ')
                                 tar -czvf  myapp-${helmversion}.tgz myapp/
                                 curl -u admin:$docker_password http://52.66.76.180:8081/repository/helm-hosted/ --upload-file myapp-${helmversion}.tgz -v
                            '''
                          }
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
