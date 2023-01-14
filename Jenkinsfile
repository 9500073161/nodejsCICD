pipeline{
    agent any
    tools {
        terraform 'terraform'
    }
    stages{

          stage('checkout source'){
            steps{
                git 'https://github.com/9500073161/9500073161-CICD-NodeJS.git'
            }
        }
        
          stage('Terraform init'){
            steps{
                sh 'terraform init'
            }
        }
          stage('Terraform Apply'){
            steps{
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: "aws-jenkins",
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                sh 'terraform apply --auto-approve'
                }
            }
        }
        
        
              
        
     }
}