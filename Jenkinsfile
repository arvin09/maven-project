pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging') {
            steps{
                build job: 'pipeline-code-deploy-to-staging'
            }
        }
        stage('Deploy to Production') {
            steps{
                timeout(time:5, unit: 'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }
                build job: 'pipeline-code-deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo 'Deployment failed.'
                }
            }
        }
    }
}