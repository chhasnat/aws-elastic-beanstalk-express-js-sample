pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    stages {
        stage('Install') {
            steps {
                sh 'npm install --save'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Security Scan') {
            steps {
                sh 'npm audit --audit-level high'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    def image = docker.build("your-dockerhub-username/app:${BUILD_NUMBER}")
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        def image = docker.image("your-dockerhub-username/app:${BUILD_NUMBER}")
                        image.push()
                    }
                }
            }
        }
    }
}
