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
                sh '''
                    echo "Checking running containers..."
                    docker ps

                    echo "Checking application ports..."
                    ss -ltn | grep -E ':3001|:3002|:3003' || true
                '''
            }
        }
    }

    post {

        success {
            emailext(
                subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
Jenkins Build Successful

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Status: SUCCESS

Docker Services:
Auth Service    : Port 3001
Order Service   : Port 3002
Product Service : Port 3003

Build URL:
${env.BUILD_URL}
""",
                to: "YOUR_EMAIL@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
Jenkins Build Failed

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Status: FAILURE

Please check the Jenkins Console Output.

Build URL:
${env.BUILD_URL}
""",
                to: "YOUR_EMAIL@gmail.com"
            )
        }

        always {
            echo "Jenkins pipeline finished with status: ${currentBuild.currentResult}"
        }
    }
}
