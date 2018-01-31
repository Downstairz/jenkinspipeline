pipeline {
    agent any

    tools {
        maven 'localmaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.216.247.90', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.217.122.14', description: 'Production Server')
    }

    /* trigger directive */
    triggers {
         pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
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
 
        stage ('Deployments') {
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "ssh -o StrictHostKeyChecking=No -i /Users/Shared/Jenkins/tomcat-demo.pem ec2-user@${params.tomcat_dev} uptime"
                        sh "scp -i /Users/Shared/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "ssh -o StrictHostKeyChecking=No -i /Users/Shared/Jenkins/tomcat-demo.pem ec2-user@${params.tomcat_dev} uptime"
                        sh "scp -i /Users/Shared/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}