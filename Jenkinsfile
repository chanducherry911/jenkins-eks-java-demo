pipeline {
    agent {
        kubernetes {
            label 'maven-agent'
        }
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/YOUR_USERNAME/java-eks-jenkins-demo.git'
            }
        }

        stage('Build App') {
            steps {
                container('maven') {
                    sh 'mvn clean package'
                }
            }
        }

    }
}
