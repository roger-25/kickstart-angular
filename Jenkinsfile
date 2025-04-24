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
    stage('Install AWS CLI') {
    steps {
        sh '''
            # Install unzip locally if not already available
            which unzip || (apt-get update && apt-get install -y unzip)

            # Download and install AWS CLI to workspace
            curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
            unzip -o awscliv2.zip
            ./aws/install -i $WORKSPACE/aws-cli -b $WORKSPACE/bin

            # Verify version
            $WORKSPACE/bin/aws --version
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
        publishS3(
            profileName: 'Roger',
            entries: [[
                sourceFile: 'dist/kickstart-angular/**',
                bucket: 'kickstar-angular',
                selectedRegion: 'us-east-1',
                storageClass: 'STANDARD',
                noUploadOnFailure: false,
                flatten: false,
                showUploadConsole: true
            ]],
            userMetadata: []
        )
    }
}

    }
}
