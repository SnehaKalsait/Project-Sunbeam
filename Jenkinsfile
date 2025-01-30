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
        SONAR_TOKEN = credentials('sqp_2b07b21f0b91ef66a6ebdd8d54110820a07cd03c') // Fetch the SonarQube token from Jenkins credentials
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
                        -Dsonar.token=${sqp_2b07b21f0b91ef66a6ebdd8d54110820a07cd03c}
                    """
                }
            }
        }
        
    }
}
