pipeline {
    
    tools {
        maven 'localMaven'
    }    
    
    agent any
    stages{
        stage('Build'){
            steps {
                    sh "mvn clean package"
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Deploy to Staging'){
            steps {
                build job: 'Deploy-to-staging'
            }
        }
        stage('SCA'){
            steps {
                build job: 'static analysis'
            }
        }
        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}