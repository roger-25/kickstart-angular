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
                sh 'ls -la dist/' // Optional: to check build output
            }
        }

        stage('Deploy to S3') {
            steps {
                sh '''
                    # Set up AWS CLI with your credentials or profile
                    export AWS_ACCESS_KEY_ID="your-access-key-id"
                    export AWS_SECRET_ACCESS_KEY="your-secret-access-key"
                    export AWS_DEFAULT_REGION="us-east-1"


                    # Deploy to S3
                    aws s3 sync dist/* s3://jenkins-kickstart/
                '''
            }
        }
    }
}
