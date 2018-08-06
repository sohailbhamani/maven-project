pipeline {
    
    environment {
        def mvn_version = 'localMaven'
    }
    agent any
    stages{
        stage('Build'){
            steps {
                
                withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                    //sh "mvn clean package"
                }
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
    }
}