pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages {
        // NPM dependencies
        stage('Pull NPM dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build image with the updated tag
                    def imageTag = "V1.${BUILD_NUMBER}"
                    docker.build("456618395112.dkr.ecr.us-east-1.amazonaws.com/doingsecr:${imageTag}")
                }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 456618395112.dkr.ecr.us-east-1.amazonaws.com/doingsecr:V1.${BUILD_NUMBER}'
            }
        }
        stage('Push to ECR') {
            steps {
                script {
                    // Define the image tag
                    def imageTag = "V1.${BUILD_NUMBER}"
                    
                    // Use the updated image tag in the withRegistry and build steps
                    docker.withRegistry('https://456618395112.dkr.ecr.us-east-1.amazonaws.com/doingsecr:latest', 'ecr:us-east-1:Doings-AWS-ECR-CREDENTIALS') {
                        // Build image with the updated tag
                        def myImage = docker.build("456618395112.dkr.ecr.us-east-1.amazonaws.com/doingsecr:${imageTag}")
                        // Push image
                        myImage.push()
                    }
                }
            }
        }
    }
}
