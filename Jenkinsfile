pipeline {
    agent any

    tools {
        maven 'Maven3'  
    }

    environment {
        IMAGE_NAME = 'backend-app'
        IMAGE_TAG = 'v1'
        NEXUS_URL = 'http://23.22.151.163/:8082'  
        NEXUS_REPO = 'docker-private'  
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

        stage('Tag and Push Docker Image to Nexus') {
            steps {
                // Tag the Docker image with the Nexus repository
                sh "docker tag $IMAGE_NAME:$IMAGE_TAG $NEXUS_URL/$NEXUS_REPO/$IMAGE_NAME:$IMAGE_TAG"

                // Push the Docker image to Nexus repository
                sh "docker push $NEXUS_URL/$NEXUS_REPO/$IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f backend-ci-container || true'  // Remove old container if exists
                sh 'docker run -d -p 8080:8080 --name backend-ci-container $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }
}
