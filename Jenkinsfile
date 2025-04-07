pipeline {
    agent any

    stages {
        stage("Check Code") {
            steps {
                git branch: 'main', url: 'https://github.com/Halfdone83/CI-CD-Jenkins'
            }
        }

        stage("SetUp .Net") {
            steps {
                bat '''
                    choco install dotnet-sdk -y --version=6.0.100
                '''
            }
        }

        stage("Install nuget packages") {
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }

        stage("Build") {
            steps {
                bat 'dotnet build SeleniumIde.sln'
            }
        }

        stage("Run Tests") {
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', 
			allowEmptyArchive: true
			step([
				$class: 'MSTestPublisher',
				testResultsFile: '**/TestResults/*.trx'
			])
            
        }
    }
}
