pipeline {
    agent any

    environment {
        PATH = "${HOME}/.local/bin:${env.PATH}"
    }

    tools {
        nodejs "NodeJS"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/roger-25/kickstart-angular.git'
            }
        }

        stage('Build Angular App') {
            steps {
                sh '''
                    npm install
                    npm run build
                '''
            }
        }

        stage('Publish to S3') {
            steps {
                withAWS(credentials: 'creds', region: 'ap-south-1') {
                    s3Upload(
                        bucket: 'jenkins-kickstart',
                        includePathPattern: 'dist/**',
                        workingDir: '',
                        acl: 'PublicRead'
                        )
                }
            }
        }

        stage('CloudFront Invalidation') {
            steps {
                withAWS(credentials: 'creds', region: 'ap-south-1') {
                    cloudfrontInvalidate(distribution: 'E34VI56BFMF82S', paths: ['/*'])
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
