pipeline {
    agent {
        kubernetes {
            label 'jenkins-agent'
        }
    }

    stages {

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
