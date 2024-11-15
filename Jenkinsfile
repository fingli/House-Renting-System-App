pipeline {
    agent any
    
    environment {
        DOTNET_VERSION = '8.0.404' // Specify the exact .NET SDK version (e.g., 8.0.100)
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
                    // Use PowerShell to set up .NET with a specified version
                    powershell '''
                        Invoke-WebRequest -Uri https://dot.net/v1/dotnet-install.ps1 -OutFile dotnet-install.ps1
                        .\\dotnet-install.ps1 -Version ${env.DOTNET_VERSION}
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
