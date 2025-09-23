pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t jenkins-project:latest .'
            }
        }

        stage('Test') {
            steps {
                echo "Running basic test..."
                // Add test commands here if needed
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying container..."
                sh '''
                docker stop jenkins-project-container || echo "No container to stop"
                docker rm jenkins-project-container || echo "No container to remove"
                docker run -d -p 3000:3000 --name jenkins-project-container jenkins-project:latest
                '''
            }
        }
    }
}
