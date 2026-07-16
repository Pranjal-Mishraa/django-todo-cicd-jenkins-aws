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
                python3 -m venv venv
                . venv/bin/activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run tests') {
            steps {
                sh '''
                . venv/bin/activate
                cd app
                python manage.py test
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
