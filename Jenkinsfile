pipeline {
    agent any
    
    stages {
        stage('SCM') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'dotnet test --no-build --verbosity normal'
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
