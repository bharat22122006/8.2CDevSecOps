pipeline {
    agent any

    environment {
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
        SONAR_TOKEN = credentials('Jenkins-token')
        SONAR_SCANNER = "C:\\sonar-scanner\\bin\\sonar-scanner.bat"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/bharat22122006/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                bat '"%SONAR_SCANNER%" -Dsonar.projectKey=bharat22122006_8.2CDevSecOps -Dsonar.organization=bharat22122006 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=%SONAR_TOKEN% -X'
            }
        }
    }
}
