pipeline {
    agent any

    environment {
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
        SONAR_TOKEN = credentials('SONAR_TOKEN')
        SONAR_SCANNER = "C:\\Users\\DELL\\OneDrive\\Documents\\Downloads\\sonar-scanner-cli-8.0.1.6346-windows-x64\\bin\\sonar-scanner.bat"
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
                bat '"%SONAR_SCANNER%" -Dsonar.token=%SONAR_TOKEN% -X'
            }
        }
    }
}
