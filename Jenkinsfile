pipeline {
    agent any
 
    tools {
        nodejs 'Node20'
    }

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'fedemo-frontend'
        CLOUDFRONT_DISTRIBUTION_ID = 'E2XA265HNVJBXJ' 
    }

    stages {

        stage('Clone Frontend Repo') {
            steps {
                git url: 'https://github.com/Mukesh-Kanna-M/fe-demo.git', branch: 'main'
            }
        }

        stage('Check Node Version') {
            steps {
                sh '''
                node -v
                npm -v
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        tage('Upload Build to S3') {
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    sh 'aws s3 sync build/ s3://$S3_BUCKET --delete'
                }
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    sh '''
                    aws cloudfront create-invalidation \
                    --distribution-id $CLOUDFRONT_DISTRIBUTION_ID \
                    --paths "/*"
                    '''
                }
            }
        }
    }

    ost {
        always {
            echo "Pipeline finished"
        }
        success {
            echo "Deployment successful "
        }
        failure {
            echo "Pipeline failed "
        }
    }
}
