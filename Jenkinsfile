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
                sh '/Users/JuanHernandez/Documents/Java/apache-maven-3.5.4/bin/mvn clean package'
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
                        sh "scp -i /home/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
