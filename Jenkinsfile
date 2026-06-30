pipeline {
    agent any

    environment {
        IMAGE_NAME = "demo-ci"
        CONTAINER_NAME = "demo-ci-container"
        PORT = "8081"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:Lilace121/demo-ci.git',
                    credentialsId: 'github-ssh'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                echo "开始构建 Docker 镜像..."
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                echo "停止旧容器（如果存在）..."
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                echo "启动新容器..."
                docker run -d \
                    --name $CONTAINER_NAME \
                    -p $PORT:80 \
                    $IMAGE_NAME
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                echo "检查容器状态..."
                docker ps | grep $CONTAINER_NAME || true
                '''
            }
        }
    }

    post {
        success {
            echo "✅ 部署成功"
        }
        failure {
            echo "❌ 部署失败"
        }
    }
}
