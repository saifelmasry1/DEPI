pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'elmaasry/my-app'  // الاسم الكامل للصورة على Docker Hub
    }

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
                script {
                    echo 'Building Docker image'
                    sh "docker build -t $DOCKER_IMAGE ."

                    echo 'Logging in and pushing to Docker Hub'
                    withCredentials([usernamePassword(credentialsId: 'my-docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push $DOCKER_IMAGE
                        """
                    }
                }
            }
        }
    }
}
