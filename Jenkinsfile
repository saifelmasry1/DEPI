pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the Java application'
                sh './mvnw clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing the Java application'
                sh './mvnw test'
            }
        }

        stage('Docker Build and Push') {
            steps {
                echo 'Building and pushing Docker image'
                sh 'docker build -t my-app .'
                sh 'docker push my-app'
            }
        }
    }
}

