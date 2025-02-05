pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB')
        
    }

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/SnehaKalsait/Project-Sunbeam.git'
            }
        }
        
          stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('WebServer-Sonar') {  // Use the SonarQube server configuration name you defined in Jenkins
                        sh """
                        sonar-scanner \
                            -Dsonar.projectKey=WebServer \
                            -Dsonar.sources=./index.html
                        """
                    }
                }
            }
        }


        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t snehakalsait/webserver .'
            }
        }

        stage('Dockerhub Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push to Dockerhub') {
            steps {
                sh 'docker push snehakalsait/webserver'
            }
        }
        
        stage('Run SSH Command') {
            steps {
                sshagent(['Project']) {
                    sh """
                ssh -o StrictHostKeyChecking=no ubuntu@3.109.122.212 "
                    echo 'Hello from Jenkins' && hostname
                    docker stop website1
                    docker pull snehakalsait/webserver:latest
                    docker rm website1
                    docker run -d --name website1 -p 80:80 snehakalsait/webserver:latest
                "
                """
                }
            }
        }
    }
}
