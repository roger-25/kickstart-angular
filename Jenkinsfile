pipeline {
    agent any
    environment {
        NODE_VERSION = 'v14.19.0'
        NPM_VERSION = '8.5.2'
        ANGULAR_CLI_VERSION = '12.2.16'
        NVM_DIR = "${WORKSPACE}/.nvm"
    }
    stages {
        stage('Setup Node & Angular CLI') {
            steps {
                sh '''
                    # Install nvm
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
                    export NVM_DIR="${NVM_DIR}"
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"

                    # Install Node.js and use it
                    . "$NVM_DIR/nvm.sh"
                    nvm install ${NODE_VERSION}
                    nvm use ${NODE_VERSION}
                    nvm alias default ${NODE_VERSION}

                    # Install npm specific version
                    npm install -g npm@${NPM_VERSION}

                    # Install Angular CLI
                    npm install -g @angular/cli@${ANGULAR_CLI_VERSION}
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    export NVM_DIR="${NVM_DIR}"
                    [ -s "$NVM_DIR/nvm.sh" ] && \\. "$NVM_DIR/nvm.sh"
                    . "$NVM_DIR/nvm.sh"
                    nvm use ${NODE_VERSION}
                    npm install
                '''
            }
        }
    }
}

