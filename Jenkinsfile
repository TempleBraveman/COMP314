pipeline {
    agent any

    environment {
        IMAGE_NAME = 'your-dockerhub-username/simple-webpage'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    script {
                        docker.withRegistry('', 'dockerhub-creds') {
                            docker.image("${IMAGE_NAME}").push()
                        }
                    }
                }
            }
        }

        stage('Run Container') {
            steps {
                sh "docker run -d -p 80:80 ${IMAGE_NAME}"
            }
        }
    }
}
