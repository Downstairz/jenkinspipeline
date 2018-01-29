pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'sudo mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}