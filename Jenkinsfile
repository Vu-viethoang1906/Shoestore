pipeline {
    agent any
    
    tools {
        'DotNet' 'dotnet'
    }
    
    stages {
        stage('SCM') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the project...'
                script {
                    try {
                        sh 'dotnet --version'
                        sh 'dotnet build'
                    } catch (Exception e) {
                        echo 'Build failed: ' + e.getMessage()
                        currentBuild.result = 'FAILURE'
                        error('Build stage failed')
                    }
                }
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                script {
                    try {
                        sh 'dotnet test --no-build --verbosity normal'
                    } catch (Exception e) {
                        echo 'Tests failed: ' + e.getMessage()
                        currentBuild.result = 'FAILURE'
                        error('Test stage failed')
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
