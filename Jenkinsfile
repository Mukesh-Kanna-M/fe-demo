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

        stage('Upload Build to S3') {
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-credentials', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
    // some block
} {
                    sh 'aws s3 cp index.html s3://$S3_BUCKET'
                }
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-credentials', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
    // some block
} {
                    sh '''
                    aws cloudfront create-invalidation \
                    --distribution-id $CLOUDFRONT_DISTRIBUTION_ID \
                    --paths "/*"
                    '''
                }
            }
        }
    }

    post {
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
