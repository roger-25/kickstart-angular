node {
    stage('Clone Repo') {
        git url: 'https://github.com/roger-25/kickstart-angular.git'
    }

    stage('Set up NodeJS') {
        // This assumes NodeJS is installed on the agent already
        // Or you can manually source NVM or set PATH here
        sh '''
            export NVM_DIR="$HOME/.nvm"
            [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
            nvm use node
        '''
    }

    stage('Install Dependencies') {
        sh 'npm install'
    }

    stage('Build Angular App') {
        sh 'npm run build'
    }

    stage('Run Tests') {
        sh 'npm test || echo "Tests failed, but continuing..."'
    }

    stage('Archive Build') {
        archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
    }
}

