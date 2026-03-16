pipeline {
    agent {
        kubernetes {
            label 'maven-agent'
        }
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/chanducherry911/jenkins-eks-java-demo.git'
            }
        }

        stage('Build Java App') {
            steps {
                container('maven') {
                    sh 'mvn -version'
                    sh 'mvn clean package'
                }
            }
        }

    }
}
