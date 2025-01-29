pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    git 'https://github.com/SnehaKalsait/Project-Sunbeam.git'
                    sh 'docker build -t mywebsite .'
                }
            }
        }
        
        environment {
        SONAR_TOKEN = credentials('53b579046bbf95c5f94c927af53557f544e262f9') // Fetch the SonarQube token from Jenkins credentials
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube Scanner
                    sh """
                    /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarScanner/bin/sonar-scanner \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.projectKey=WebServer \
                        -Dsonar.sources=./index.html \  # Specify the file/folder you want to analyze
                        -Dsonar.token=${53b579046bbf95c5f94c927af53557f544e262f9}
                    """
                }
            }
        }
        
    }
}
