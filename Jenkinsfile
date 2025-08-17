pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "skfarid45/e-commerce-site"
        DOCKER_CREDENTIALS = credentials('dockerhub')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/skfarid45/e-Commerce-Site.git'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'clean package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_HUB_REPO:$BUILD_NUMBER .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh "echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'docker push $DOCKER_HUB_REPO:$BUILD_NUMBER'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Docker Image pushed: $DOCKER_HUB_REPO:$BUILD_NUMBER"
        }
        failure {
            echo "❌ Build Failed!"
        }
    }
}
