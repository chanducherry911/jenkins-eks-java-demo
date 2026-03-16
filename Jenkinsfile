pipeline {
    agent {
        kubernetes {
            inheritFrom 'jenkins-agent'
        }
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
                          --destination 131127508996.dkr.ecr.ap-south-1.amazonaws.com/java-app-demo-eks:latest
                    '''
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                container('kubectl') {
                    sh '''
                        kubectl create namespace java-app --dry-run=client -o yaml | kubectl apply -f -

                        kubectl apply -n java-app -f deployment.yaml

                        kubectl apply -n java-app -f service.yaml
                    '''
                }
            }
        }

    }
}
