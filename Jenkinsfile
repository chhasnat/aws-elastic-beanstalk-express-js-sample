pipeline {
  agent {
    docker {
      image 'node:16'
    }
  }
  stages {
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
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t Project2-Compose_jenkins_1 .'
      }
    }
    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
          sh 'docker push Project2_Compose_jenkins_1'
        }
      }
    }
    stage('Security Scan') {
      steps {
        sh 'npm install -g snyk'
        sh 'snyk test --severity-threshold=high'
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      junit '**/test-results.xml'
    }
    failure {
      mail to: 'team@example.com',
           subject: 'Pipeline failed',
           body: 'Check Jenkins for details.'
    }
  }
}
