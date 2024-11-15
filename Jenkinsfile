pipeline {
    agent any
    environment {
        DOTNET_VERSION = '8.0.x' // Specify the .NET version
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Setup .NET') {
            steps {
                script {
                    // Use the 'sh' step to set up .NET
                    sh '''
                        wget https://dot.net/v1/dotnet-install.sh
                        chmod +x dotnet-install.sh
                        ./dotnet-install.sh --channel ${DOTNET_VERSION}
                        export PATH=$HOME/.dotnet:$PATH
                    '''
                }
            }
        }
        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }
        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }
        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}