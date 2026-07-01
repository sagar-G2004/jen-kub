pipeline {
    agent any

    environment {
        IMAGE_NAME = "sagarg23/demo-image"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Docker Login') {
    steps {
        withCredentials([
            usernamePassword(
                credentialsId: 'sagar',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )
        ]) {
            sh '''
                echo "$DOCKER_PASS" | docker login \
                    -u "$DOCKER_USER" \
                    --password-stdin
            '''
        }
    }
}

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    kubectl apply -f deployment.yaml
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    kubectl get deployments
                    kubectl get pods
                    kubectl get svc
                '''
            }
        }
    }
}
