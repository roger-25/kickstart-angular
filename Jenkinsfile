pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        PATH = "${env.NODE_HOME}/bin:${env.PATH}"
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo 'Cleaning old build artifacts...'
                sh 'rm -rf dist/ node_modules/'
            }
        }

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/roger-25/kickstart-angular.git', branch: 'master'
            }
        }

        stage('Install Angular CLI (if required)') {
            steps {
                sh 'npm install -g @angular/cli'
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
                sh 'ls -la dist/' // Optional: Verify build output
            }
        }
        stage('Install AWS CLI') {
          steps {
              sh '''
                  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                  yes | unzip -o awscliv2.zip
                  sudo ./aws/install
              '''
          }
        }
        stage('Push to S3') {
            steps {
                    sh 'aws s3 sync dist/* s3://jenkins-kickstart/ --delete'
                }
            }
        }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed. Please check the console output.'
        }
    }
}
