pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('WebServer-Sonar') // Fetch the SonarQube token from Jenkins credentials
    }

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/SnehaKalsait/Project-Sunbeam.git'
                sh 'docker build -t mywebsite .'
            }
        }

         stage('SonarQube Analysis') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'WebServer-Sonar', variable: 'SONAR_TOKEN')]) {
                        sh """
                        /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarScanner/bin/sonar-scanner \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.projectKey=WebServer \
                            -Dsonar.sources=./index.html \
                            -Dsonar.token=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }
        
        stage('Run SSH Command') {
            steps {
                sshCommand remote: [
                    name: 'EC2-Server',      
                    host: '3.109.122.212',
                    user: 'ubuntu',
                    credentialsId: 'Project',
                    allowAnyHosts: true
                ], command: 'echo "Hello from Jenkins" && hostname'
            }
        }
    }
}
