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
                sh '''
                    npm install

                    # Install AWS CLI v2 with sudo
                    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                    sudo unzip -o awscliv2.zip
                    sudo ./aws/install
                '''
            }
        }

        stage('Build Angular App') {
            steps {
                sh '''
                    npm run build
                    ls -la dist/
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(region: 'us-east-1', credentials: 'Roger') {
                    s3Upload(
                        file: '',
                        bucket: 'kickstar-angular',
                        includePathPattern: '**',
                        workingDir: 'dist/kickstart-angular'
                    )
                }
            }
        }
    }
}
