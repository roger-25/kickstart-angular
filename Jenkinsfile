pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        S3_BUCKET = 'jenkins-kickstart'
        CLOUDFRONT_DISTRIBUTION_ID = 'E34VI56BFMF82S'
        AWS_REGION = 'us-east-1'
        BUILD_FOLDER = 'dist/**/*'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/roger-25/kickstart-angular.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build'
            }
        }

    	stage('Upload to S3') {
            steps {
                // Use Jenkins S3 Publisher Plugin
                s3Upload(
                    bucket: "${env.S3_BUCKET}",
                    path: 'dist/',
                    includePathPattern: '**/*',
                    workingDir: 'dist',
                )
         }

        stage('Create CloudFront Invalidation') {
            steps {
                script {
                    // Create invalidation for all files
                    sh "aws cloudfront create-invalidation --distribution-id ${env.CLOUDFRONT_DISTRIBUTION_ID} --paths '/*' --region ${env.AWS_REGION}"
                }
            }
        }
    }

    post {
        success {
            echo 'Build, deployment, and invalidation completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
        }
    }
}
}