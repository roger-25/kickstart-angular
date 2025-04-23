pipeline {
    agent any

    tools {
        nodejs 'nodejs-18'   // Match the name from Global Tool Config
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build' // if needed
            }
        }
    }
}

