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
