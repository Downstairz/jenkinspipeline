pipeline {
    agent any

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
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
 
        stage ('Deployments') {
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /Users/bsoumpholphakdy/Downloads/tomcat-demo.pem **/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/bsoumpholphakdy/Downloads/tomcat-demo.pem **/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}