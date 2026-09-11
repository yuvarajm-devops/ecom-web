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
                      -p 3001:3001 \
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
                      -p 3002:3002 \
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
                      -p 3003:3003 \
                      ecom-product
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}

    post {
    success {
        emailext(
            subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Jenkins build completed successfully.\n${env.BUILD_URL}",
            to: "your-email@gmail.com"
        )
    }

    failure {
        emailext(
            subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Jenkins build failed.\n${env.BUILD_URL}",
            to: "your-email@gmail.com"
        )
    }
}
