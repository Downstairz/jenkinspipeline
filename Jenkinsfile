pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh '/Users/bsoumpholphakdy/Documents/apache-maven-3.5.2/bin/mvn clean package-2'
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