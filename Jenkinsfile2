pipeline{
    agent any
   
    stages{

          stage('checkout source'){
            steps{
                git 'https://github.com/9500073161/nodejsCICD.git'
            }
        }
        
          stage('Terraform init'){
            steps{
                dir('terraform') {
                sh 'terraform init'        
            }
                
            }
        }
          stage('Terraform Apply'){
            steps{
                dir('terraform') {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: "awsjenkins",
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                
                sh 'terraform apply --auto-approve'
                }
            }
            
            }
        }
        
        
              
        
     }
}