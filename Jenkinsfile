pipeline {
    agent any
    
    tools {
        nodejs 'NodeJS'
    }
    
    environment {
        SONAR_PROJECT_KEY = 'node'
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
    }

    stages {

        stage('Checkout Github') {
            steps {
                git branch: 'main', 
                    credentialsId: 'github-cred', 
                    url: 'https://github.com/Guerriche-ala-abdelmoumene/Simple_NodeJS_App.git'
            }
        }

        stage('Install node dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'node-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {

                        bat """
                            "%SONAR_SCANNER_HOME%\\bin\\sonar-scanner.bat" ^
                            -Dsonar.projectKey=%SONAR_PROJECT_KEY% ^
                            -Dsonar.sources=. ^
                            -Dsonar.host.url=http://192.168.1.128:9000 ^
                            -Dsonar.login=%SONAR_TOKEN%
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs.'
        }
    }
}
