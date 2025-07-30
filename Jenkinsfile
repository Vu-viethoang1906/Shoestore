pipeline {
    agent any
    
    stages {
        stage("SCM") {
            steps {
                echo "Checking out source code..."
                checkout scm
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