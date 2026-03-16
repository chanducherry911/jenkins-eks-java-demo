pipeline {
    agent {
        kubernetes {
            label 'maven-agent'
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
