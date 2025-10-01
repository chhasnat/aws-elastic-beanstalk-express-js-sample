pipeline {
    agent {
        docker { image 'node:16' }   // Use Node.js 16 Docker image as agent
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install --save'   // Install Node.js dependencies
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test'             // Run unit tests
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t chhasnat/node-app .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push chhasnat/node-app:latest
                    '''
                }
            }
        }
    }
}
