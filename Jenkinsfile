pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Auth Image') {
            steps {
                sh 'docker build -t ecom-auth ./auth-service'
            }
        }

        stage('Build Order Image') {
            steps {
                sh 'docker build -t ecom-order ./order-service'
            }
        }

        stage('Build Product Image') {
            steps {
                sh 'docker build -t ecom-product ./product-service'
            }
        }

        stage('Deploy Auth') {
            steps {
                sh '''
                    docker rm -f ecom-auth 2>/dev/null || true

                    docker run -d \
                        --name ecom-auth \
                        -p 5001:3001 \
                        ecom-auth
                '''
            }
        }

        stage('Deploy Order') {
            steps {
                sh '''
                    docker rm -f ecom-order 2>/dev/null || true

                    docker run -d \
                        --name ecom-order \
                        -p 5002:3002 \
                        ecom-order
                '''
            }
        }

        stage('Deploy Product') {
            steps {
                sh '''
                    docker rm -f ecom-products 2>/dev/null || true

                    docker run -d \
                        --name ecom-products \
                        -p 5003:3003 \
                        ecom-product
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    echo "Checking running containers..."
                    docker ps

                    echo "Checking application ports..."
                    ss -ltn | grep -E ':5001|:5002|:5003' || true
                '''
            }
        }
    }

    post {

        success {
            emailext(
                subject: "Jenkins Build",
                body: "SUCCESS",
                to: "yuvarajm.ops@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "Jenkins Build",
                body: "FAILED",
                to: "yuvarajm.ops@gmail.com"
            )
        }

        always {
            script {
                if (currentBuild.currentResult == 'SUCCESS') {
                    echo "SUCCESS"
                } else {
                    echo "FAILED"
                }
            }
        }
    }
}
