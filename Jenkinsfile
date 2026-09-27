pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/YOUR_USERNAME/docker-smoke-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t smoke-demo .
                '''
            }
        }

        stage('Remove Existing Container') {
            steps {
                sh '''
                    docker rm -f smoke-test || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker run -d \
                        --name smoke-test \
                        -p 8080:80 \
                        smoke-demo
                '''
            }
        }

        stage('Smoke Test') {
            steps {
                sh '''
                    sleep 5
                    curl -f http://localhost:8080
                '''
            }
        }
    }

    post {

        success {
            echo 'Docker application passed the smoke test!'
        }

        failure {
            echo 'Pipeline failed!'
        }

        always {
            sh 'docker rm -f smoke-test || true'
            echo 'Pipeline execution completed.'
        }
    }
}
