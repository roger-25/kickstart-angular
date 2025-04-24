pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        PATH = "${env.NODE_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/roger-25/kickstart-angular.git', branch: 'master'
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
                sh 'ls -la dist/' // Optional
            }
        }

        // No need to install AWS CLI if using IAM role for EC2
        stage('Deploy to S3') {
            environment {
                AWS_DEFAULT_REGION = 'us-east-1' // Optional: Set default region
            }
            steps {
                sh '''
                    echo "Deploying to S3..."
                    if [ -d "dist" ]; then
                        # Use IAM role to access AWS resources
                        aws s3 sync dist/* s3://kickstar-angular/ --delete
                    else
                        echo "dist directory not found"
                        exit 1
                    fi
                '''
            }
        }
    }
}
