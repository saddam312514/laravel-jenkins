
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code from the repository
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Build your PHP application (e.g., run composer, compile assets, etc.)
                sh 'composer update && composer install'
                sh 'sh 'php artisan build'
                // Add more build steps as needed
            }
        }
        stage('Archive Artifacts') {
            steps {
                // Archive build artifacts (e.g., compiled code, assets, etc.)
                archiveArtifacts artifacts: '**/build/*', allowEmptyArchive: true
            }
        }
         stage('Deploy to Production'){
             steps{
                 timeout(time:5, unit:'DAYS'){
                     input message:'Approve PRODUCTION Deployment?'
                 }
                 build job: 'Deploy_Application_Prod_Env'
             }
         }
    }
}
