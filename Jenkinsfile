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
                            # Install .NET SDK using curl
                            curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel 8.0
                            export PATH="$HOME/.dotnet:$PATH"
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
                        sh '''
                            export PATH="$HOME/.dotnet:$PATH"
                            dotnet build
                        '''
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
                        sh '''
                            export PATH="$HOME/.dotnet:$PATH"
                            dotnet test --no-build --verbosity normal
                        '''
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