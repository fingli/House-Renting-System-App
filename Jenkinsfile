pipeline {
    agent {
        label 'windows-latest' // Replace with the actual label of your Windows agent
    }
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
                    // Use PowerShell to set up .NET on Windows
                    powershell '''
                        Invoke-WebRequest -Uri https://dot.net/v1/dotnet-install.ps1 -OutFile dotnet-install.ps1
                        .\\dotnet-install.ps1 -Channel ${env.DOTNET_VERSION}
                        $env:PATH="$env:USERPROFILE\\.dotnet;$env:PATH"
                    '''
                }
            }
        }
        stage('Restore Dependencies') {
            steps {
                powershell 'dotnet restore'
            }
        }
        stage('Build') {
            steps {
                powershell 'dotnet build --no-restore'
            }
        }
        stage('Test') {
            steps {
                powershell 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}
