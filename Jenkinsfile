pipeline {
    agent any

    tools {
        maven 'Maven 3.9.9'  // Name must match Jenkins tool config 
    }

    environment {
        IMAGE_NAME = 'backend-app'
        IMAGE_TAG = 'v1'
    }

    stages {
        stage('Clone Code') {
            steps {
                git url: 'https://github.com/farhanfist10/backend-maven.git', branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8080:8080 --name backend-ci-container $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }
}
