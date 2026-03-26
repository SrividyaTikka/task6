pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-id') // stored in Jenkins
        DOCKER_IMAGE = "srividyatikka/task6:latest"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SrividyaTikka/task6.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $DOCKER_IMAGE'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                # Configure EKS access
                aws eks --region ap-south-1 update-kubeconfig --name task6

                # Verify connection (optional but recommended)
                kubectl get nodes

                # Deploy resources
                kubectl apply -f k8s/deployment.yml
                kubectl apply -f k8s/service.yml
                kubectl apply -f k8s/ingress.yml
                '''
            }
        }
    }
}
