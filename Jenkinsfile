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
    }
}
