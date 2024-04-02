pipeline {
    agent any

    tools {
        maven "maven3.9.6"
    }

    stages {
        stage ("git clone") {
            steps {
                git 'https://github.com/SirDominique/maven-build-website.git'
            }
        }

        stage ("build with maven") {
            steps {
                sh '''
                mvn clean
                mvn test
                mvn package
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                ScannerHome = tool 'sonar5.0'
            }
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        sh "${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=jomacs"
                    }
                }
            }
        } 

    }
}