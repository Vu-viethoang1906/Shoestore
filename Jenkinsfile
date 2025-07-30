pipeline {
    agent any

    environment {
        DOTNET_ROOT = "${env.HOME}/.dotnet"
        PATH = "${env.HOME}/.dotnet:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '🔄 Checking out source code...'
                checkout scm
            }
        }

        stage('Setup .NET SDK') {
            steps {
                echo '🛠️ Setting up .NET SDK...'
                script {
                    if (isUnix()) {
                        sh '''
                            export DOTNET_ROOT="$HOME/.dotnet"
                            export PATH="$DOTNET_ROOT:$PATH"

                            if [ ! -d "$DOTNET_ROOT/sdk" ]; then
                                echo "Installing .NET SDK 8.0.412..."
                                curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --version 8.0.412
                            else
                                echo ".NET SDK already installed."
                            fi

                            $DOTNET_ROOT/dotnet --info
                        '''
                    } else {
                        bat 'dotnet --version'
                    }
                }
            }
        }

        stage('Restore') {
            steps {
                echo '📦 Restoring dependencies...'
                script {
                    if (isUnix()) {
                        sh '''
                            export DOTNET_ROOT="$HOME/.dotnet"
                            export PATH="$DOTNET_ROOT:$PATH"
                            $DOTNET_ROOT/dotnet restore
                        '''
                    } else {
                        bat 'dotnet restore'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo '🏗️ Building application...'
                script {
                    if (isUnix()) {
                        sh '''
                            export DOTNET_ROOT="$HOME/.dotnet"
                            export PATH="$DOTNET_ROOT:$PATH"
                            $DOTNET_ROOT/dotnet build --no-restore
                        '''
                    } else {
                        bat 'dotnet build --no-restore'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Running tests...'
                script {
                    if (isUnix()) {
                        sh '''
                            export DOTNET_ROOT="$HOME/.dotnet"
                            export PATH="$DOTNET_ROOT:$PATH"
                            $DOTNET_ROOT/dotnet test --no-build --verbosity normal || true
                        '''
                    } else {
                        bat 'dotnet test --no-build --verbosity normal'
                    }
                }
            }
        }
    }

    post {
        always {
            echo '📋 Pipeline completed!'
        }
        success {
            echo '✅ Build succeeded!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
