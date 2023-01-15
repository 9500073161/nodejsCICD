pipeline{
    agent any

    environment{
   //Kubernetes for credentails
        ACCESS_KEY = credentials('AWS_ACCESS_KEY_ID')
        SECRET_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }
   
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
        
        stage('docker image building'){

                steps{

                    script{
                        
                        dir('nodeweb') {
    // some block

                        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID ponnu2014/$JOB_NAME:v1.$BUILD_ID'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID ponnu2014/$JOB_NAME:latest'
                    }
                }
            }
         
      
          }   

         stage('Push Image to DockerHub'){

                steps{

                    script{

                        withCredentials([string(credentialsId: 'dockerHub_passwd', variable: 'dockerHub_passwd')]) {
                            
                            sh 'docker login -u ponnu2014 -p ${dockerHub_passwd}'
                            sh 'docker image push ponnu2014/$JOB_NAME:v1.$BUILD_ID'
                            sh 'docker image push ponnu2014/$JOB_NAME:latest'
                      }
                    }
                }
            }
        stage('Connect with Cluster'){

        steps{

            script{

                sh """
                aws configure set aws_access_key_id "$ACCESS_KEY"
                aws configure set aws_secret_access_key "$SECRET_KEY"
                aws configure set region "us-east-1"                                                   
                aws eks --region us-east-1 update-kubeconfig --name nodejs-eks-cluster-1
                """
                
                
            }
        }
    } 
         
          stage('kubernetes deployment'){

        steps{

            script{

                sh """
                   kubectl apply -f deploymentservice.yml
                """
                
                
            }
        }
    } 
              
        
     }
}