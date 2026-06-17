pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Django Check') {
            steps {
                sh '''
                . venv/bin/activate
                python manage.py check
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t django-app .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f django-container || true
                docker run -d --name django-container -p 8000:8000 django-app
                '''
            }
        }
    }
}