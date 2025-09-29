pipeline {
    agent any
    
    stages {
        stage('Setup Node.js') {
            steps {
                sh '''
                    # Install Node.js 16
                    curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
                    apt-get install -y nodejs
                    node --version
                    npm --version
                '''
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install --save'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        stage('Security Scan') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    sh 'npm audit --audit-level high'
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh '''
                    echo "FROM node:16" > Dockerfile
                    echo "WORKDIR /app" >> Dockerfile  
                    echo "COPY package*.json ./" >> Dockerfile
                    echo "RUN npm install" >> Dockerfile
                    echo "COPY . ." >> Dockerfile
                    echo "EXPOSE 3000" >> Dockerfile
                    echo "CMD [\\"npm\\", \\"start\\"]" >> Dockerfile
                    docker build -t chhasnat/myapp:${BUILD_NUMBER} .
                '''
            }
        }
        stage('Success') {
            steps {
                echo 'Pipeline completed successfully!'
                sh 'ls -la'
            }
        }
    }
}
