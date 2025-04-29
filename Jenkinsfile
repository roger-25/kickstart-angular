pipeline {
    agent any

    environment {
        NODE_HOME = tool name: 'NodeJS', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        PATH = "${NODE_HOME}/bin:${env.PATH}"
        AWS_DEFAULT_REGION = 'us-east-1'
    }


        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

      stages {
        stage ('Sonarqube scan'){
           steps{
            script {
              scannerHome = tool 'Sonar'
                }
                withSonarQubeEnv('Sonar') {
              sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=kickstart-angular -Dsonar.sources=src"
                  }
                }
              }
      stage('Download SonarQube Report') {
    steps {
        script {
            def sonarHost = 'http://34.205.18.219:9000'
            def projectKey = 'kickstart-angular'
            def authToken = 'squ_4d436fa3da2f841eeff6c9c8c7c8e745045e8f41'

            // Download Issues Report (JSON format)
            sh """
                curl -u ${authToken}: \
                '${sonarHost}/api/issues/search?componentKeys=${projectKey}&resolved=false' \
                -o sonar-report.json
            """

            // Archive the JSON report (optional)
            archiveArtifacts artifacts: 'sonar-report.json', fingerprint: true
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
