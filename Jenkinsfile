pipeline {
    agent {
        label 'windows' // Replace with the actual label of your Windows agent
    }
    environment {
        DOTNET_VERSION = '8.0.11' // Specify the exact .NET SDK version
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
                    // Use PowerShell to set up .NET with the specified version
                    powershell '''
                        $url = "https://dot.net/v1/dotnet-install.ps1"
                        $output = "dotnet-install.ps1"
                        
                        # Download the script
                        Invoke-WebRequest -Uri $url -OutFile $output -UseBasicParsing

                        # Run the script with the exact version
                        .\\dotnet-install.ps1 -Version ${env.DOTNET_VERSION} -InstallDir $env:USERPROFILE\\.dotnet

                        # Add .NET to PATH
                        $env:PATH="$env:USERPROFILE\\.dotnet;$env:PATH"
                        Write-Host "PATH is now: $env:PATH"
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
