pipeline { 
    agent any 
    environment { 
        IMAGE_NAME = "pravindaskrishnasamy/spring-boot-hello-devops" 
        IMAGE_TAG = "1.0.0" 
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
                sh 'mvn clean install -DskipTests' 
            } 
        } 
        stage('Test') { 
            steps { 
                sh 'mvn test' 
            } 
        } 
        stage('Package') { 
            steps { 
                sh 'mvn package -DskipTests' 
            } 
        } 
        stage('Docker Build & Push') { 
            steps { 
                script { 
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) { 
                        sh """ 
                        docker build -t $IMAGE_NAME:$IMAGE_TAG . 
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin 
                        docker push $IMAGE_NAME:$IMAGE_TAG 
                        """ 
                    } 
                } 
            } 
        } 
        stage('Deploy to Kubernetes') { 
            steps { 
                sh ''' 
                /usr/local/bin/kubectl --kubeconfig=/Users/pravin/.kube/config apply -f k8s-deployment.yaml --validate=false
                /usr/local/bin/kubectl --kubeconfig=/Users/pravin/.kube/config apply -f k8s-service.yaml --validate=false
                /usr/local/bin/kubectl --kubeconfig=/Users/pravin/.kube/config get pods
                /usr/local/bin/kubectl --kubeconfig=/Users/pravin/.kube/config get svc
                ''' 
            } 
        } 
    } 
}
