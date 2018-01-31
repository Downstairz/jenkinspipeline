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
    }
}