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
        
stage('Install AWS CLI') {
    steps {
        sh '''
            # Install AWS CLI locally without sudo
            if ! command -v aws &> /dev/null; then
                echo "Installing AWS CLI..."
                curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                # Use Python to extract if unzip isn't available
                python3 -c "import zipfile; zipfile.ZipFile('awscliv2.zip').extractall('.')"
                ./aws/install -i $HOME/aws-cli -b $HOME/bin
                export PATH=$HOME/bin:$PATH
                aws --version
            fi
        '''
    }
}
        stage('Upload to S3') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${env.AWS_REGION}") {
                    sh "aws s3 sync dist/ s3://${env.S3_BUCKET} --delete"
                }
            }
        }
        stage('Create CloudFront Invalidation') {
            steps {
                withAWS(credentials: 'aws-creds', region: "${env.AWS_REGION}") {
                    sh "aws cloudfront create-invalidation --distribution-id ${env.CLOUDFRONT_DISTRIBUTION_ID} --paths '/*'"
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
        }
    }
}