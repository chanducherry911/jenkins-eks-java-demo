pipeline {
    agent {
        kubernetes {
            inheritFrom 'jenkins-agent'
        }
    }

    environment {
        AWS_REGION   = "ap-south-1"
        CLUSTER_NAME = "rapy-eks-cluster"
        ECR_REPO     = "131127508996.dkr.ecr.ap-south-1.amazonaws.com/java-app-demo-eks"
    }

    stages {

        stage('Build Java App') {
            steps {
                container('maven') {
                    sh '''
                        mvn clean package
                    '''
                }
            }
        }

        stage('Build and Push Image') {
            steps {
                container('kaniko') {
                    sh '''
                        /kaniko/executor \
                        --dockerfile Dockerfile \
                        --context . \
                        --destination ${ECR_REPO}:latest
                    '''
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                container('kubectl') {
                    sh '''
                        aws eks update-kubeconfig --region ${AWS_REGION} --name ${CLUSTER_NAME}

                        kubectl create namespace java-app --dry-run=client -o yaml | kubectl apply -f -

                        kubectl apply -n java-app -f deployment.yaml
                        kubectl apply -n java-app -f service.yaml

                        kubectl get pods -n java-app
                    '''
                }
            }
        }

    }
}
