pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }
    environment {
        S3_BUCKET = 'jenkins-kickstart'
        CLOUDFRONT_DISTRIBUTION_ID = 'E34VI56BFMF82S'
        AWS_REGION = 'us-east-1'
        AWS_HOME = "${env.HOME}/.aws"
        PATH = "${env.HOME}/.local/bin:${env.PATH}"
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
                    if ! command -v aws &> /dev/null; then
                        echo "Installing AWS CLI..."
                        mkdir -p ${HOME}/.local/aws-cli
                        curl -s "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

                        if python3 -c "import zipfile; zipfile.ZipFile('awscliv2.zip').extractall('.')"; then
                            echo "Extracted with Python"
                        elif command -v unzip &> /dev/null; then
                            unzip awscliv2.zip
                        else
                            echo "Error: No extraction method available"
                            exit 1
                            fi

                        chmod -R u+rwX aws
                        chown -R $(whoami) aws
                        chmod +x ./aws/install

                ./aws/install -i ${HOME}/.local/aws-cli -b ${HOME}/.local/bin --install-dir ${WORKSPACE}/.aws-cli-tmp
                aws --version || exit 1
            fi
        '''
    }
}

        stage('Configure AWS Credentials') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh '''
                        mkdir -p ${HOME}/.aws
                        cat > ${HOME}/.aws/config <<EOF
[default]
region = ${AWS_REGION}
output = json
EOF
                        cat > ${HOME}/.aws/credentials <<EOF
[default]
aws_access_key_id = ${AWS_ACCESS_KEY_ID}
aws_secret_access_key = ${AWS_SECRET_ACCESS_KEY}
EOF
                        chmod 600 ${HOME}/.aws/*
                    '''
                }
            }
        }
        stage('Upload to S3') {
            steps {
                sh "aws s3 sync dist/ s3://${env.S3_BUCKET} --delete"
            }
        }
        stage('Create CloudFront Invalidation') {
            steps {
                sh "aws cloudfront create-invalidation --distribution-id ${env.CLOUDFRONT_DISTRIBUTION_ID} --paths '/*'"
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