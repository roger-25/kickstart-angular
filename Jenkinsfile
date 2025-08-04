pipeline {
    agent any
    tools {
      nodejs 'NodeJS' 
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/roger-25/kickstart-angular.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm run build'
            }
        }
    }
}
