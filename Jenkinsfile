pipeline {
    agent any

    environment {
        DOTNET_ROOT = "${env.HOME}/.dotnet"
        PATH = "${env.HOME}/.dotnet:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Setup .NET') {
            steps {
                echo 'Setting up .NET environment...'
                script {
                    if (isUnix()) {
                        sh '''
                            echo "Checking if dotnet is installed..."
                            if [ ! -f "$DOTNET_ROOT/dotnet" ]; then
                                echo "Installing .NET SDK..."
                                curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel 8.0
                            fi

                            echo ">>> Kiểm tra dotnet:"
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
                echo 'Restoring dependencies...'
                script {
                    if (isUnix()) {
                        sh '$DOTNET_ROOT/dotnet restore'
                    } else {
                        bat 'dotnet restore'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
                script {
                    if (isUnix()) {
                        sh '$DOTNET_ROOT/dotnet build --no-restore'
                    } else {
                        bat 'dotnet build --no-restore'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                script {
                    if (isUnix()) {
                        sh '$DOTNET_ROOT/dotnet test --no-build --verbosity normal'
                    } else {
                        bat 'dotnet test --no-build --verbosity normal'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed!'
        }
        success {
            echo '✅ Build succeeded!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
