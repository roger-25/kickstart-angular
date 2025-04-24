node {
    def nodeHome = tool name: 'NodeJS', type: 'NodeJS installations'
    env.PATH = "${nodeHome}/bin:${env.PATH}"

    stage('Check Node Version') {
        sh 'node -v'
        sh 'npm -v'
    }

    stage('Install Angular CLI') {
        sh 'npm install -g @angular/cli'
    }

    stage('Install Project Dependencies') {
        sh 'npm install'
    }

    stage('Build Angular App') {
        sh 'npm run build'
    }
}

