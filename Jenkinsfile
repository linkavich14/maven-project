pipeline {
    agent any
    parameters {
        string(name: 'tomcat_dev', defaultValue: '54.210.63.240', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '35.172.138.158', description: 'Production Server')
    }
    triggers {
        pollSCM('* * * * *') // * * * * 1-5 , 0 * * * * , 0,30 * * * *
    }

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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    /*
                    steps {
                        bat "scp -i C:/Users/Juan/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                    */
                    steps {
                        bat 'set Path=%Path%;C:\\Users\\Juan\\Downloads\\tomcat-demo.pem'
                        bat 'scp -i "tomcat-server-kp.ppk" **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp -i C:/Users/Juan/Downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
