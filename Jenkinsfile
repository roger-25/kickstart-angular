pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        S3_BUCKET = 'jenkins-kickstart'
        CLOUDFRONT_DISTRIBUTION_ID = 'E34VI56BFMF82S'
        AWS_REGION = 'us-east-1'
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
                s3Upload(
                    file: 'dist',
                    bucket: "${env.S3_BUCKET}",
                    path: '/',
                    noUploadOnFailure: true,
                    uploadFromSlave: false,
                    managedArtifacts: false
                )
            }
        }

        stage('Create CloudFront Invalidation') {
            steps {
                script {
                    // First check if AWS CLI is available
                    sh '''
                        if ! command -v aws &> /dev/null; then
                            echo "AWS CLI not found, installing..."
                            curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                            unzip awscliv2.zip
                            sudo ./aws/install
                        fi
                    '''
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