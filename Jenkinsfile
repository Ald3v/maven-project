pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        
        stage ('Deploy to Staging'){
            
            parallel{
                stage ('Deploy-to-staging'){
                    steps {
                        build job: 'Deploy-to-staging'
                    }
                 } 
                
                stage ('Code Quality'){
                steps {
                    build job: 'static analysis'
                }
             } 
           }
        }
       

        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'Deploy-to-Prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }


    }
}
