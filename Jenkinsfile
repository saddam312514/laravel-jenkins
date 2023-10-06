
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
                //sh 'composer update && composer install'
                sh 'npm install'
                sh 'npm run production'
            }
        }
   // stage('Archive as ZIP') {
   //          steps {
   //              // Archive the Laravel project as a ZIP file using the full path
   //              archiveArtifacts allowEmptyArchive: true, artifacts: '**/*'
   //          }
   //      }

             stage('Archive as ZIP') {
            steps {
                // Archive the Laravel project as a ZIP file
                archiveArtifacts allowEmptyArchive: true, artifacts: '**/*', excludes: ''
                // script {
                //     // Rename the archived ZIP file to a specific name (optional)
                //     def zipFileName = "/var/lib/jenkins/workspace/Package_install_Laravel.zip"
                //     sh "mv *zip ${zipFileName}"
                // }
            }
        }

         stage('Deploy in Staging Environment'){
            steps{
                 build job: 'Deploy_Application_Staging_laravel'
                
                 sh 'sudo composer update && sudo composer install'
                 sh 'sudo cp -r /var/lib/jenkins/workspace/.env /var/lib/jenkins/workspace/Deploy_Application_Staging_laravel/'
                 sh 'sudo chown -R www-data:www-data /var/lib/jenkins/workspace/Deploy_Application_Staging_laravel/'
                 sh 'sudo chmod -R 775 /var/lib/jenkins/workspace/Deploy_Application_Staging_laravel/storage'
                 // def sourceDir = '/var/lib/jenkins/workspace/Deploy_Application_Staging_laravel/'
                 // def destinationDir = '/var/www/html/blog'
                // Copy files from source to destination
                //sh "cp -rp ${sourceDir}/* ${destinationDir}/"

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
