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

        withCredentials([usernamePassword(
            credentialsId: 'jira-creds',
            usernameVariable: 'JIRA_USER',
            passwordVariable: 'JIRA_TOKEN'
        )]) {
            bat """
            curl -X POST ^
              -u %JIRA_USER%:%JIRA_TOKEN% ^
              -H "Content-Type: application/json" ^
              --data "{\\"body\\": \\"Build SUCCESS from Jenkins\\"}" ^
              https://jcarrete.atlassian.net/rest/api/3/issue/NOTES-58/comment
            """
        }
    }

    failure {
        echo 'Build FAILED'

        withCredentials([usernamePassword(
            credentialsId: 'jira-creds',
            usernameVariable: 'JIRA_USER',
            passwordVariable: 'JIRA_TOKEN'
        )]) {
            bat """
            curl -X POST ^
              -u %JIRA_USER%:%JIRA_TOKEN% ^
              -H "Content-Type: application/json" ^
              --data "{\\"body\\": \\"Build FAILED from Jenkins\\"}" ^
              https://jcarrete.atlassian.net/rest/api/3/issue/NOTES-58/comment
            """
        }
    }
}
