pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-app'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/VaibhavSingh149/prac_del'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker run -d -p 5000:5000 --name ${IMAGE_NAME}-container $IMAGE_NAME"
            }
        }
    }

    post {
        always {
            echo 'Cleaning up containers...'
            sh 'docker stop flask-app-container || true'
            sh 'docker rm flask-app-container || true'
        }
        failure {
            echo '❌ Build failed!'
        }
        success {
            echo '✅ Build succeeded!'
        }
    }
}
