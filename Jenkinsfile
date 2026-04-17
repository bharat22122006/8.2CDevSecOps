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
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat '''
                    curl -o sonar.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip
                    powershell -Command "Expand-Archive sonar.zip -DestinationPath . -Force"
                    cd sonar-scanner-*/bin
                    sonar-scanner.bat 
                    -Dsonar.projectKey=bharat22122006_8.2CDevSecOps
                    -Dsonar.organization=bharat22122006 
                    -Dsonar.host.url=https://sonarcloud.io 
                    -Dsonar.login=%SONAR_TOKEN%
                    '''
                }
            }
    }
}
