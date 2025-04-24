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

        stage('Install AWS CLI') {
            steps {
                sh '''
                    # Run AWS CLI Docker container to install AWS CLI
                    docker run --rm -v $PWD:/workspace -w /workspace amazon/aws-cli \
                        curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" &&
                        unzip -o awscliv2.zip &&
                        ./aws/install -i $WORKSPACE/aws-cli -b $WORKSPACE/bin

                    # Verify AWS CLI installation
                    $WORKSPACE/bin/aws --version
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                publishS3(
                    bucket: 'kickstar-angular',
                    entries: [[
                        sourceFile: 'dist/kickstart-angular/**',
                        destinationBucket: '',
                        storageClass: 'STANDARD'
                    ]],
                    profileName: 'Roger',
                    userMetadata: []
                )
            }
        }
    }
}
