pipeline {
    agent none
    environment {
        DOTNET_CLI_HOME = "/tmp/dotnet_cli_home"
    }

    stages {
        stage('C# Build and test') {
            agent {
                docker { image 'mcr.microsoft.com/dotnet/sdk:6.0' }
            }
            steps {
                checkout scm
                sh "dotnet build"
                sh "dotnet test"
            }
        }

        stage('TS Build and test') {
            agent {
                docker { image 'node:17-bullseye' }
            }
            steps {
                checkout scm
                sh "npm install --prefix ./DotnetTemplate.Web"
                sh "npm run lint --prefix ./DotnetTemplate.Web"
                sh "npm run test-with-coverage --prefix ./DotnetTemplate.Web"

                publishCoverage (failUnhealthy: true, 
                        globalThresholds: [[thresholdTarget: 'Report', unhealthyThreshold: 90.0]],
                        adapters: [istanbulCoberturaAdapter(path: 'DotnetTemplate.Web/coverage/cobertura-coverage.xml')])
            }
        }
    }
}