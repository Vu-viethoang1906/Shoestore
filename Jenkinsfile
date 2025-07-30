pipeline {
    agent any

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
                            if ! command -v dotnet &> /dev/null; then
                                echo "Installing .NET SDK..."
                                curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel 8.0
                                export PATH="$HOME/.dotnet:$PATH"
                            fi
                            dotnet --version
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
                        sh 'dotnet restore'
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
                        sh 'dotnet build --no-restore'
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
                        sh 'dotnet test --no-build --verbosity normal'
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
