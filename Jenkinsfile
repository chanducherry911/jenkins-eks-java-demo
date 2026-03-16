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
                        set -e

                        curl -LO https://dl.k8s.io/release/v1.29.0/bin/linux/amd64/kubectl
                        chmod +x kubectl
                        mv kubectl /usr/local/bin/

                        aws eks update-kubeconfig --region ${AWS_REGION} --name ${CLUSTER_NAME}                       

                        kubectl apply -n java-app -f deployment.yaml
                        kubectl apply -n java-app -f service.yaml

                        kubectl get pods -n java-app
                    '''
                }
            }
        }

    }
}
