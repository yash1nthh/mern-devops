pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'compose', url: 'https://github.com/yash1nthh/mern-devops.git'
            }
        }

        stage('Apply Docker Compose and tag docker image') {
            steps {
                bat '''
                    docker compose up -d
                    docker tag mern-frontend yash6422/mern:frontend
                    docker tag mern-backend yash6422/mern:backend
                '''
            }
        }

        stage('Login to Docker Hub and Push Images') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker1') {
                        docker.image('yash6422/mern:frontend').push()
                        docker.image('yash6422/mern:backend').push()
                    }
                }
            }
        }
         stage('Deploy to Kubernetes') {
            steps {
                script {
                    dir('k8s') {
                        bat '''
                            kubectl apply -f deployment.yaml
                            kubectl apply -f service.yaml
                            kubectl get pods
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
