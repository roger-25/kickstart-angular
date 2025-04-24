node {
    def nodeHome
    def repo = 'https://github.com/roger-25/kickstart-angular.git'
    def branch = 'master'
    def s3Bucket = 'kickstar-angular'

    stage('Setup Node.js') {
        nodeHome = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        env.PATH = "${nodeHome}/bin:${env.PATH}"
        sh 'node -v'
        sh 'npm -v'
    }

    stage('Checkout') {
        git url: repo, branch: branch
    }

    // Install dependencies
    stage('Install Dependencies') {
        sh 'npm install'
    }

    // Build Angular application
    stage('Build Angular App') {
        sh 'npm run build'
        sh 'ls -la dist/'  // Optional: For verification
    }

    // Deploy the dist/ directory to S3
    stage('Deploy to S3') {
        // Use Jenkins credentials for AWS access
        withCredentials([usernamePassword(credentialsId: 'creds',
                                          usernameVariable: 'AWS_ACCESS_KEY_ID',
                                          passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
            // Set the AWS region and run the sync command
            withEnv(['AWS_DEFAULT_REGION=us-east-1']) {
                sh '''
                    echo "Starting S3 sync..."
                    # Ensure dist/ exists before syncing
                    if [ -d "dist" ]; then
                        aws s3 sync dist/ s3://${s3Bucket}/ --delete
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
