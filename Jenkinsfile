pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        PATH = "${NODE_HOME}/bin:${env.PATH}"
        AWS_DEFAULT_REGION = 'us-east-1'
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
