pipeline {
    agent any

    tools {
        nodejs 'nodejs-14.19.0'   // Match the name from Global Tool Config
    }

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                
            }
        }
    }
}

