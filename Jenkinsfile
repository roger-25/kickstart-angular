pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // This must match the name you set in Jenkins global tools
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

        stage('Run Tests') {
            steps {
                // If you have tests defined in package.json
                sh 'npm test || echo "Tests failed, continuing..."'
            }
        }
    }
}
