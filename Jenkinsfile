pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('List files') {
            steps {
                sh 'ls -R'
            }
        }
    }
}