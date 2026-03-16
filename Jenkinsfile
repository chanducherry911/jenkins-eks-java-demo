pipeline {
    agent {
        kubernetes {
            inheritFrom 'jenkins-agent'
        }
    }

    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "131127508996.dkr.ecr.ap-south-1.amazonaws.com/java-app-demo-eks"
    }

    stages {

        stage('Build Java App') {
            steps {
                container('maven') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build and Push Image') {
            steps {
                container('kaniko') {
                    sh '''
                    /kaniko/executor \
                      --dockerfile Dockerfile \
                      --context $(pwd) \
                      --destination $ECR_REPO:latest
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
