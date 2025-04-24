node {
    // Use the NodeJS tool defined in Global Tool Configuration
    def nodeHome = tool name: 'NodeJS', type: 'NodeJSInstallations'
    env.PATH = "${nodeHome}/bin:${env.PATH}"  // Add NodeJS to PATH

    stage('Clone Repo') {
        git url: 'https://github.com/roger-25/kickstart-angular.git'
    }

    stage('Install Dependencies') {
        sh 'npm install'
    }

    stage('Build Angular App') {
        sh 'npm run build'
    }
}

