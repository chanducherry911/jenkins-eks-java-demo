pipeline {
    agent {
        kubernetes {
            label 'jenkins-agent'
        }
    }

    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "131127508996.dkr.ecr.ap-south-1.amazonaws.com/java-app-demo-eks"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Build Java App') {
            steps {
                container('maven') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                container('docker') {
                    sh '''
                    docker build -t java-app .
                    docker tag java-app:latest $ECR_REPO:$IMAGE_TAG
                    '''
                }
            }
        }

        stage('Login to ECR') {
            steps {
                container('docker') {
                    sh '''
                    aws ecr get-login-password --region $AWS_REGION \
                    | docker login --username AWS --password-stdin $ECR_REPO
                    '''
                }
            }
        }

        stage('Push Image to ECR') {
            steps {
                container('docker') {
                    sh '''
                    docker push $ECR_REPO:$IMAGE_TAG
                    '''
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh '''
                kubectl create namespace demo --dry-run=client -o yaml | kubectl apply -f -
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }

    }
}
