pipeline {
    agent any // Allows the pipeline to run on any available agent
    environment {
        DOTNET_VERSION = '8.0.100' // Specify the exact .NET SDK version
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
                    // Use PowerShell on Windows or Bash on Linux/Mac
                    if (isUnix()) {
                        sh '''
                            wget https://dot.net/v1/dotnet-install.sh
                            chmod +x dotnet-install.sh
                            ./dotnet-install.sh --version ${DOTNET_VERSION}
                            export PATH=$HOME/.dotnet:$PATH
                        '''
                    } else {
                        powershell '''
                            Invoke-WebRequest -Uri https://dot.net/v1/dotnet-install.ps1 -OutFile dotnet-install.ps1
                            .\\dotnet-install.ps1 -Version ${env.DOTNET_VERSION} -InstallDir $env:USERPROFILE\\.dotnet
                            $env:PATH="$env:USERPROFILE\\.dotnet;$env:PATH"
                        '''
                    }
                }
            }
        }
        stage('Restore Dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'dotnet restore'
                    } else {
                        powershell 'dotnet restore'
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'dotnet build --no-restore'
                    } else {
                        powershell 'dotnet build --no-restore'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'dotnet test --no-build --verbosity normal'
                    } else {
                        powershell 'dotnet test --no-build --verbosity normal'
                    }
                }
            }
        }
    }
}
