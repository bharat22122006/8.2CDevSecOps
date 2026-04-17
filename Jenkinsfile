pipeline {
    agent any

    environment {
        PATH = "C:\\Program Files\\nodejs;${env.PATH}"
        SONAR_TOKEN = credentials('SONAR_TOKEN')
        SCANNER_HOME = "${WORKSPACE}\\sonar-scanner"
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
        bat '''
        if not exist sonar-scanner (
            powershell -Command "Invoke-WebRequest -Uri https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-5.0.1.3006-windows.zip -OutFile sonar-scanner.zip"
            powershell -Command "Expand-Archive -Path sonar-scanner.zip -DestinationPath ."
            for /d %%i in (sonar-scanner-*) do ren "%%i" sonar-scanner
        )

        sonar-scanner\\bin\\sonar-scanner.bat -Dsonar.scanner.skipJreProvisioning=true
        '''
    }
}
    }
}
