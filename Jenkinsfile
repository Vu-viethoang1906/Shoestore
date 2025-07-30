pipeline {
    agent any
    
    stages {
        stage("SCM") {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }
        
        stage("Install .NET") {
            steps {
                echo "Installing .NET SDK..."
                script {
                    if (isUnix()) {
                        sh '''
                            # Download and install .NET SDK
                            wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
                            sudo dpkg -i packages-microsoft-prod.deb
                            sudo apt-get update
                            sudo apt-get install -y apt-transport-https
                            sudo apt-get install -y dotnet-sdk-8.0
                            dotnet --version
                        '''
                    } else {
                        bat '''
                            # For Windows, assume .NET is already installed or use winget
                            dotnet --version
                        '''
                    }
                }
            }
        }
        
        stage("Build") {
            steps {
                echo "Building the application..."
                script {
                    if (isUnix()) {
                        sh "dotnet build"
                    } else {
                        bat "dotnet build"
                    }
                }
            }
        }
        
        stage("Test") {
            steps {
                echo "Running tests..."
                script {
                    if (isUnix()) {
                        sh "dotnet test --no-build --verbosity normal"
                    } else {
                        bat "dotnet test --no-build --verbosity normal"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo "Build succeeded!"
        }
        failure {
            echo "Build failed!"
        }
    }
} 