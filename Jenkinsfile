pipeline {
    agent any
    environment {
       BUILDYML = 'buildImage.yml'
       ANSIBLE_YML1 = 'pullNexus_pushAzurecr.yml'
       PULLDEPLOYYML = 'pulldeployImage.yml'
    }
    stages {
        stage('mvn compile') {
            steps {
                script {
                    
                    mvn.compile() 
                    
                }
            }
        }
        stage('mvn test') {
            steps {
                script {
                    
                    mvn.test()
                    
                }
            }
        }

        stage('mvn verify/sonar') {
            steps {
                script {
                    
                    mvn.verify()
                    
                }
            }
        }

        stage('Package and deploy to Nexus') {
            steps{
                configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]){ 
                    script {

                        mvn.deploy()
                        
                    } 
                }
            }
        }
    
     stage('Push Image') {
            steps {
              withCredentials([usernamePassword(credentialsId: 'AZURECR', usernameVariable: 'AZURECR_USER', passwordVariable: 'AZURECR_PASSWORD')]) {
                
                script {
                    
                  ansibleplay.imagepullpush(ANSIBLE_YML1)
                    
                }
              }
            }
            
        }
        stage('deploy with ansible') {
            steps {
               script {
                    
                  ansibleplay.imagepullpush(ANSIBLE_YML1)
                    
                }
            }
        }
        
    }
}
