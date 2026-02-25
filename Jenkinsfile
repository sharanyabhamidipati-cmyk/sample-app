pipeline {
    agent any

    tools {
        maven 'Maven-3'
    }

    environment {
    	IMAGE_NAME      = "sample-app"
    	IMAGE_TAG       = "1.0"
    	AWS_ACCOUNT_ID  = "558684556050"
    	AWS_REGION      = "us-east-1"
    	ECR_REGISTRY    = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
    	ECR_REPOSITORY  = "sample-app"
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

        stage('Push to ECR') {
            steps {
        	sh '''
        	aws ecr get-login-password --region $AWS_REGION | \
        	docker login --username AWS --password-stdin $ECR_REGISTRY

        	docker tag $IMAGE_NAME:$IMAGE_TAG $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

        	docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG
        	'''
            }
        }
    }

    post {
        success {
            echo "Docker image pushed to ECR successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
