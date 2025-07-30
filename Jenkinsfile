pipeline {
    agent any
    
    stages {
        stage("SCM") {
            steps {
                echo "Checking out source code..."
                checkout scm
            }
        }
        
        stage("Check .NET") {
            steps {
                echo "Checking if .NET is available..."
                script {
                    if (isUnix()) {
                        sh '''
                            # Check if .NET is already installed
                            if command -v dotnet &> /dev/null; then
                                echo ".NET is already installed"
                                dotnet --version
                            else
                                echo ".NET not found, trying to install..."
                                # Try using curl instead of wget
                                if command -v curl &> /dev/null; then
                                    curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel 8.0
                                    export PATH="$HOME/.dotnet:$PATH"
                                    $HOME/.dotnet/dotnet --version
                                else
                                    echo "Neither wget nor curl available. Using Docker approach..."
                                    # Use Docker to build
                                    docker run --rm -v $(pwd):/app -w /app mcr.microsoft.com/dotnet/sdk:8.0 dotnet --version
                                fi
                            fi
                        '''
                    } else {
                        bat "dotnet --version"
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
                            # Try to build with available .NET
                            if command -v dotnet &> /dev/null; then
                                dotnet build
                            else
                                # Use Docker to build
                                docker run --rm -v $(pwd):/app -w /app mcr.microsoft.com/dotnet/sdk:8.0 dotnet build
                            fi
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
                            # Try to test with available .NET
                            if command -v dotnet &> /dev/null; then
                                dotnet test --no-build --verbosity normal
                            else
                                # Use Docker to test
                                docker run --rm -v $(pwd):/app -w /app mcr.microsoft.com/dotnet/sdk:8.0 dotnet test --no-build --verbosity normal
                            fi
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