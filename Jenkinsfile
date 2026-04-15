pipeline {
    agent any
    environment {
        S3_BUCKET = 'fedemo-frontend'
        AWS_REGION = 'us-east-1'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
            }
        }
        stage('Deploy to S3') {
            steps {
                withAWS(credentials: 'aws-credentials',
                        region: 'us-east-1') {
                    s3Upload(
                        bucket: 'fedemo-frontend',
                        path: '',
                        includePathPattern: '**/*',
                        excludePathPattern: '.git/**'
                    )
                }
            }
        }
    }
}
