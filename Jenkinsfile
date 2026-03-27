pipeline {
    agent any

    environment {
        PYTHON = "C:\\Users\\rober\\AppData\\Local\\Programs\\Python\\Python314\\python.exe"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Debug Files') {
            steps {
                bat 'dir'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'set PYTHONIOENCODING=utf-8'
                bat '"%PYTHON%" -m pip install -r requirements.txt'
            }
        }

        stage('Run App') {
            steps {
                bat '"%PYTHON%" app.py'
            }
        }

        stage('Run Tests') {
            steps {
                bat '"%PYTHON%" -m pytest'
            }
        }
    }

    post {
        success {
            echo 'Build SUCCESS'
        }

        failure {
            echo 'Build FAILED'
        }
    }
}
