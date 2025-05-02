pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building the Java application'
                sh './mvnw clean package'
            }
        }
        stage ('test') {
            steps {
                echo 'Testing the Java application'
                sh './mvnw test'
            }
        }
        stage ('Docker Build and Push') {
            steps {
                script {
                    echo 'Building and pushing to Docker hub'
                    // بناء الصورة باستخدام الاسم الجديد للمستودع
                    docker.build("elmaasry/app-test:jenkins-test")

                    // الدفع إلى مستودع Docker Hub الخاص بك
                    docker.withRegistry('https://index.docker.io/v1/', 'my-docker-hub') {
                        docker.image("elmaasry/app-test:jenkins-test").push()
                    }
                }
            }        
        }
        stage('Docker Run') {
            steps {
                dir('Ansible') {
                    script {
                        ansiblePlaybook credentialsId: 'ansible-ssh', disableHostKeyChecking: true, installation: 'ansible', inventory: '/opt/ansible/ansible-demo/', playbook: 'playbook.yml'
                    }
                }
            }
        }
    }
}

