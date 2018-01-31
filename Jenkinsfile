pipeline {
    agent any
    tools {
        maven 'localmaven'
    }
    stages{
        stage('Build'){
            steps {
                sh '/usr/local/Cellar/maven/3.5.2/bin/mvn clean package'
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