pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"
        AWS_CLI_VERSION = '2.13.28'
        REPO = 'https://github.com/roger-25/kickstart-angular.git'
        BRANCH = 'master'
        S3_BUCKET = 'kickstar-angular'
        AWS_DEFAULT_REGION = 'us-east-1'
    }

    stages {
        stage('Verify Node.js & NPM') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }

        stage('Checkout') {
            steps {
                git url: "${env.REPO}", branch: "${env.BRANCH}"
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
                sh 'ls -la dist/'
            }
        }

        stage('Install AWS CLI') {
    steps {
        sh '''
            echo "Downloading AWS CLI tar.gz instead of zip to avoid unzip dependency..."
            curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.tar.gz" -o "awscliv2.tar.gz"
            tar -xzf awscliv2.tar.gz
            ./aws/install -i $WORKSPACE/aws-cli -b $WORKSPACE/aws-cli-bin
            export PATH=$WORKSPACE/aws-cli-bin:$PATH
            aws --version
        '''
    }
}

        stage('Deploy to S3') {
            environment {
                AWS_DEFAULT_REGION = 'us-east-1'
            }
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'creds',
                    usernameVariable: 'AWS_ACCESS_KEY_ID',
                    passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                )]) {
                    sh '''
                        echo "Starting S3 sync..."
                        if [ -d "dist" ]; then
                            aws s3 sync dist/ s3://${S3_BUCKET}/ --delete
                            echo "S3 sync complete!"
                        else
                            echo "dist/ directory does not exist. Skipping S3 deployment."
                            exit 1
                        fi
                    '''
                }
            }
        }
    }
}
