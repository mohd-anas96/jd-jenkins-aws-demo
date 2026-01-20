pipeline {
    agent any

    environment {
        S3_BUCKET = "jd-jenkins-demo-artifacts-2345"
        LAMBDA_NAME = "jd-jenkins-demo-lambda"
        AWS_REGION = "us-east-1"
        ARTIFACT = "lambda.zip"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r $ARTIFACT app.py'
            }
        }

        stage('Upload to S3') {
            steps {
                sh 'aws s3 cp $ARTIFACT s3://$S3_BUCKET/$ARTIFACT --region $AWS_REGION'
            }
        }

        stage('Deploy to Lambda') {
            steps {
                sh '''
                aws lambda update-function-code \
                --region $AWS_REGION \
                --function-name $LAMBDA_NAME \
                --s3-bucket $S3_BUCKET \
                --s3-key $ARTIFACT
                '''
            }
        }
    }
}
