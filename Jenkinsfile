pipeline {
    agent any
    environment {
        NODE_VERSION = 'v14.19.0'
        NPM_VERSION = '8.5.2'
        ANGULAR_CLI_VERSION = '12.2.16'
    }
    stages {
        stage('Setup Node & Angular CLI') {
            steps {
                sh '''
                    # Install Node.js
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
                    nvm install $NODE_VERSION
                    nvm use $NODE_VERSION
                    nvm alias default $NODE_VERSION

                    # Install specific npm version
                    npm install -g npm@$NPM_VERSION

                    # Install Angular CLI
                    npm install -g @angular/cli@$ANGULAR_CLI_VERSION
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    export NVM_DIR="$HOME/.nvm"
                    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
                    nvm use $NODE_VERSION
                    npm install
                '''
            }
        }
    }
}

