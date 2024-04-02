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

                    stage ('Upload to Nexus') {
                        steps {
                            nexusArtifactUploader artifacts: [[artifactId: 'earth-app', classifier: '', file: '/var/lib/jenkins/workspace/maven-build-website/target/earth-app-1.0-SNAPSHOT.war', type: 'war']], credentialsId: 'nexus-id', groupId: 'com.devops.maven', nexusUrl: '35.94.144.94:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'mavenwebapp-snapshot', version: '1.0-SNAPSHOT'
                        }
                    }
                }
            }
        } 

        stage ('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://34.219.120.95:8080/')], contextPath: null, war: 'target/*.war'
            }
        }

    }
}