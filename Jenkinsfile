pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("012010923904.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:v1")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 012010923904.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:v1'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://012010923904.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:v1', 'ecr:eu-east-1:anne-acr') {
                    // build image
                    def myImage = docker.build("012010923904.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:v1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
