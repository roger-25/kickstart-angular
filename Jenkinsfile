pipeline {
    agent any
<<<<<<< HEAD
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
=======

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
>>>>>>> 8742d3b77c307de5da5b9ccdd5b1bbb21436e5c6
            }
        }
    }
}
