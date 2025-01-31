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
                    sh """
                    /var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarScanner/bin/sonar-scanner \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.projectKey=WebServer \
                        -Dsonar.sources=./index.html \
                        -Dsonar.token=${sqp_3916f89f21d6de81968f32a793c0d851479933ef}
                    """
                }
            }
        }

        stage('Run SSH Command') {
            steps {
                sshCommand remote: [host: '13.233.199.84', user: 'ubuntu', credentialsId: 'Ec2-SSH'], command: 'echo "Hello from Jenkins" && hostname'
            }
        }
    }
}
