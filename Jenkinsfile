pipeline {
    agent any

      environment {
    NODE_HOME = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
    // Sets NODE_HOME to the installation path of a NodeJS tool configured in Jenkins (Manage Jenkins > Global Tool Configuration).
    // 'NodeJS' is the name of that tool.

    PATH = "${NODE_HOME}/bin:${env.PATH}"
    // Updates the system PATH variable to include NodeJS binaries so you can run commands like `node` or `npm` directly in shell steps.

    AWS_DEFAULT_REGION = 'us-east-1'
    // Sets the default AWS region for any AWS CLI commands or SDKs used in the pipeline to 'us-east-1'.
}


      stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Sonarqube scan'){           // Defines a pipeline stage named 'Sonarqube scan'
    steps {
        script {
            scannerHome = tool 'Sonar'   // Fetches the path of the SonarQube scanner tool named 'Sonar' (configured in Jenkins > Global Tool Configuration)
        }
        withSonarQubeEnv('Sonar') {      // Sets up the SonarQube environment using credentials and server settings named 'Sonar' (configured in Jenkins > Configure System)
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=kickstart-angular -Dsonar.sources=src"
            // Runs the sonar-scanner CLI with:
            // - sonar.projectKey: Unique identifier for the project in SonarQube
            // - sonar.sources: Source code directory to analyze (here, 'src')
        }
    }
}


          
        stage('Build Angular App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Push to S3') {
            steps {
                sh 'aws s3 sync dist/kickstart-angular/ s3://jenkins-kickstart/ --delete'
            }
        }

        stage('CloudFront Invalidation') {
            steps {
                sh 'aws cloudfront create-invalidation --distribution-id E34VI56BFMF82S --paths "/*"'
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
