
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
                 // Define the source and destination directories
                
                  
                // Create the destination directory if it doesn't exist
                sh "mkdir -p /var/www/html/laravel"

                // Copy all contents from the workspace to the destination directory
                sh "cp -rp /var/lib/jenkins/workspace/Deploy_Application_Staging_laravel/* /var/www/html/laravel"
                 sh 'cd /var/www/html/laravel/sudo composer update && cd /var/www/html/laravel/sudo composer install'
                 sh 'sudo cp -r /var/lib/jenkins/workspace/.env /var/www/html/laravel/'
                 sh 'sudo chown -R www-data:www-data /var/www/html/laravel/'
                 sh 'sudo chmod -R 775 /var/www/html/laravel/storage'
                

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
