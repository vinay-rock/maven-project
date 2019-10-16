pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
       stage ('static analysis'){
            steps {
                build job: 'static analysis'
            }
        }
    }
}

        stage ('Deploy to Production Package'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'package-pipeline'
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
