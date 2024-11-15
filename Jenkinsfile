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