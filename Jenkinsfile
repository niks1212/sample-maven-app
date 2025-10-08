pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nikhil1112/sample-maven-app"
    }

    stages {
        stage('Checkout') {
            steps {
                // Explicitly checkout the main branch
                git branch: 'main', url: 'https://github.com/niks1212/sample-maven-app.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f sample-app || true'
                sh 'docker run -d --name sample-app -p 8081:8080 $DOCKER_IMAGE'
            }
        }
    }
}

