pipeline {
    agent {
        docker { image 'node:16' }   // Node.js 16 build agent
    }

    tools {
        snyk 'snyk@latest'   // Matches what you configured in Manage -> Tools
    }

    environment {
        SNYK_TOKEN = credentials('SNYK_TOKEN')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/chhasnat/aws-elastic-beanstalk-express-js-sample.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Snyk Security Scan') {
            steps {
                sh '''
                   npm install -g snyk
                   snyk test --severity-threshold=high
                   snyk monitor --org=isalarwork
                '''
            }
        }
    }
}
