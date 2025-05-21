pipeline {
  agent {
    docker {
      image 'python:3.10-slim'
      args '-u root:root'
    }
  }

  environment {
    APP_DIR = 'app'
  }

  stages {
    stage('Checkout') {
      steps {
        echo '📥 Checking out code...'
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        echo '📦 Installing Python dependencies...'
        sh 'pip install -r requirements.txt'
      }
    }

    stage('Run Tests') {
      steps {
        echo '🧪 Running unit tests...'
        sh 'pytest --junitxml=results.xml'
      }
    }

    stage('Archive Test Results') {
      steps {
        echo '📁 Archiving test results...'
        junit 'results.xml'
      }
    }

    stage('Build Docker Image') {
      steps {
        echo '🐳 Building Docker image...'
        sh 'docker build -t flask-app .'
      }
    }
  }

  post {
    success {
      echo '✅ Build and tests passed!'
    }
    failure {
      echo '❌ Something went wrong!'
    }
  }
}
