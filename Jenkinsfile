pipeline {
    agent any
    tools {
        maven 'localmaven'
    }
    stages{

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                    echo 'Archiving is completed!'
                }
            }
        }

        stage('Deploy to staging') {
            steps {
                /* a job to define in jenkins */
                build job: 'deploy-to-staging'
            }

            post {
                success {
                    echo 'Staging completed'
                }
            }
        }

        stage('Deploy to Production Yo!') {
            steps {
            /* 5 days will expire if no one approves of deployment */
                timeout(time: 5, unit: 'DAYS') {
                    input message: 'Approve PRODUCTION Deployment?', submitter: 'buddha'
                }

                build job: 'deploy-to-production'
            }

            post {
                success {
                    echo 'Production deployment completed.'
                }

                failure {
                    echo 'Production deployment failed.'
                }

            }
        }
    }
}