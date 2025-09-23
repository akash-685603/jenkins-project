pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building Docker image..."
                script {
                    docker.build("jenkins-project:latest")
                }
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
                script {
                    // Stop old container if exists
                    try {
                        docker.stop('jenkins-project-container')
                        docker.rm('jenkins-project-container')
                    } catch(Exception e) {
                        echo "No old container to remove."
                    }

                    // Run new container
                    docker.run('-d -p 3000:3000 --name jenkins-project-container jenkins-project:latest')
                }
            }
        }
    }
}
