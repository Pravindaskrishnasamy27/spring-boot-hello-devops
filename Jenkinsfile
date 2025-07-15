pipeline {
    agent any

    environment {
        IMAGE_NAME = "pravindaskrishnasamy27/spring-boot-hello-devops"
        IMAGE_TAG = "1.0.0"
        DOCKER_USERNAME = credentials('pravindas')
        DOCKER_PASSWORD = credentials('DOCKER_TOKEN')
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh '''
                    docker build -t $IMAGE_NAME:$IMAGE_TAG .
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f k8s/deployment.yaml
                    kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}
