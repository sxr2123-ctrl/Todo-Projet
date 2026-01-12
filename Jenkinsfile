pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-souhail11'
        BACKEND_IMAGE = 'souhail11/react-mongo-flask-main-backend:latest'
        FRONTEND_IMAGE = 'souhail11/react-mongo-flask-main-frontend:latest'
    }

    stages {
        stage('Build Backend') {
            steps {
                echo "🚀 Building backend Docker image..."
                sh 'docker build -t $BACKEND_IMAGE ./backend'
            }
        }

        stage('Build Frontend') {
            steps {
                echo "🚀 Building frontend Docker image..."
                sh 'docker build -t $FRONTEND_IMAGE ./frontend'
            }
        }

        stage('Push Images') {
            steps {
                echo "📤 Pushing Docker images to Docker Hub..."
                withCredentials([
                    usernamePassword(
                        credentialsId: DOCKERHUB_CREDENTIALS,
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]) {
                    sh '''
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                        docker push $BACKEND_IMAGE
                        docker push $FRONTEND_IMAGE
                        docker logout
                    '''
                }
            }
        }

        stage('Finish') {
            steps {
                echo "🎉 All builds and deployments completed successfully!"
            }
        }
    }

    post {
        always {
            echo "🔎 Containers currently running on this machine:"
            sh 'docker ps || true'
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
