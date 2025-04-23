pipeline {
    agent any

    tools {
        nodejs 'nodejs-20'   // Match the name from Global Tool Config
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
    }
}

