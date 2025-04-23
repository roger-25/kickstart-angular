pipeline {
    agent any

    tools {
        nodejs 'nodejs-14.19.0'   // Match the name you configured in Jenkins
    }

    stages {
        stage('Install & Build') {
            steps {
                sh 'node -v'
                sh 'npm -v'
                sh 'ng version'
                sh 'npm install'
            }
        }
    }
}
