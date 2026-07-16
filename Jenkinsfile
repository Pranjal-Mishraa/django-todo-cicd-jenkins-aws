pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                sh '''
                cd app
                pip3 install -r requirements.txt
                '''
            }
        }

        stage('Run tests') {
            steps {
                sh '''
                cd app
                python3 manage.py test
                '''
            }
        }

        stage('List files') {
            steps {
                sh 'ls -R'
            }
        }
    }
}
