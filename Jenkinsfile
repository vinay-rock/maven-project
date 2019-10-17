pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '13.233.198.94', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '35.154.52.61', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
       stage('user'){
          sh "echo $USER"
       }
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/ubuntu/Kub.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/ubuntu/Kub.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
