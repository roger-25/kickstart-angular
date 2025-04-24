pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        PATH = "${HOME}/.local/bin:${env.PATH}"
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
      
stage('Install AWS CLI (No sudo)') {
    steps {
        sh '''
            mkdir -p $HOME/aws-cli-install
            cd $HOME/aws-cli-install
            curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

            # Use Python to extract the ZIP if unzip is not available
            python3 -c "
import zipfile
with zipfile.ZipFile('awscliv2.zip', 'r') as zip_ref:
    zip_ref.extractall('.')
"

            ./aws/install --bin-dir $HOME/.local/bin --install-dir $HOME/.aws-cli --update
            echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc
            export PATH=$HOME/.local/bin:$PATH
            aws --version
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
