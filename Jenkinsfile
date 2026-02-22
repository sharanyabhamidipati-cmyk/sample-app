pipeline {
    agent any

    tools {
        maven 'Maven-3'
    }

    environment {
        IMAGE_NAME = "sample-app"
        IMAGE_TAG  = "1.0"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/sharanyabhamidipati-cmyk/sample-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Docker Image List') {
            steps {
                sh 'docker images'
            }
        }
    }
}
