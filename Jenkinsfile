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
                    docker.build("456618395112.dkr.ecr.us-east-1.amazonaws.com/doingsecr:V1")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 456618395112.dkr.ecr.us-east-1.amazonaws.com/doingsecr:V1'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://456618395112.dkr.ecr.us-east-1.amazonaws.com/doingsecr:latest', 'ecr:us-east-1:Doings-AWS-ECR-CREDENTIALS') {
                    // build image
                    def myImage = docker.build("456618395112.dkr.ecr.us-east-1.amazonaws.com/doingsecr:V1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
