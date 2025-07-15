pipeline {
    agent any

    tools {
        maven 'Maven 3.9.6'   // Ensure this name matches what is configured in Jenkins > Global Tools
        jdk 'Java 17'         // Same for JDK
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Pravindaskrishnasamy27/spring-boot-hello-devops.git'
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
    }
}