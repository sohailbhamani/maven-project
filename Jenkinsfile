pipeline {
    
    environment {
        def mvn_version = 'localMaven'
    }
    agent any
    stages{
        stage('Build'){
            steps {
                withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
                    sh "mvn clean package"
                }
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage("Deploy and SCA"){
            parallel {
                stage('Deploy to Staging') {
                    steps{
                        build job: 'deploy-to-staging'
                    }
                }
                stage('Static Code Analysis') {
                    steps{
                        buils job: 'static analysis'
                    }
                }
            }
        }    
    }
}