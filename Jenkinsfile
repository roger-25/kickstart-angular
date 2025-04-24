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
                script {
                    docker.image('amazon/aws-cli').inside {
                        sh '''
                            # Configure AWS CLI
                            aws configure set aws_access_key_id YOUR_ACCESS_KEY
                            aws configure set aws_secret_access_key YOUR_SECRET_KEY
                            aws configure set default.region YOUR_REGION

                            # Deploy to S3
                            aws s3 cp dist/kickstart-angular/ s3://kickstar-angular/ --recursive
                        '''
                    }
                }
            }
        }
    }
}
