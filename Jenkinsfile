node {
    stage('Clone Repo') {
        git url: 'https://github.com/roger-25/kickstart-angular.git'
    }

    stage('Check Node & NPM Version') {
        sh 'node -v'
        sh 'npm -v'
    }

    stage('Install Dependencies') {
        sh 'npm install'
    }

    stage('Build Angular App') {
        sh 'npm run build'
    }

    stage('Archive Build') {
        archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
    }
}

