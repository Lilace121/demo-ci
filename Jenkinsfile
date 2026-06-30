pipeline {
    agent any

    environment {
        IMAGE_NAME = "demo-nginx"
        CONTAINER_NAME = "demo-nginx"
        PORT = "8081"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/Lilace121/demo-ci.git',
                    branch: 'main'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Old') {
            steps {
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
            }
        }

        stage('Run') {
            steps {
                sh '''
                docker run -d \
                    --name $CONTAINER_NAME \
                    -p $PORT:80 \
                    $IMAGE_NAME
                '''
            }
        }

    }

    post {
        success {
            echo "SUCCESS"
        }
        failure {
            echo "FAILED"
        }
    }
}
