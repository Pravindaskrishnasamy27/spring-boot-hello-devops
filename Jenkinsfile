pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()      // 💣 Deletes all files in workspace
                checkout scm     // Re-checkout code after clean
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
