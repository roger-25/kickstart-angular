node {
    // Use the NodeJS tool defined in Global Tool Configuration
    def nodeHome = tool name: 'NodeJS', type: 'NodeJSInstallations'
    env.PATH = "${nodeHome}/bin:${env.PATH}"

    stage('Install Dependencies') {
        sh 'npm install'
    }

    stage('Build Angular App') {
        sh 'npm run build'
    }
}

