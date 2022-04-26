pipeline {
    agent none
    environment {
        DOTNET_CLI_HOME = "/tmp/dotnet_cli_home"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('C# Build and test') {
            agent {
                docker { image 'mcr.microsoft.com/dotnet/sdk:6.0' }
            }
            steps {
                sh "dotnet build"
                sh "dotnet test"
            }
        }
    }
}