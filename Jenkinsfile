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
        stage('Deploy to Staging and Checkstyle Concurrently'){
            steps {
                parallel {
                    stage('Deploy to Staging') {
                        agent {
                            label "staging"
                        }
                        steps{
                            build job: 'deploy-to-staging'
                        }
                    }
                    stage('Static Code Analysis') {
                        agent {
                            label "checkstyle"
                        }
                        steps{
                            buils job: 'static analysis'
                        }
                    }
                }
            }
        }
    }
}