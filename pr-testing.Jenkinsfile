pipeline {
    agent any

    stages {
        stage('Unittest') {
            steps {
                sh 'exit 5'
            }
        }
        stage('Lint') {
            steps {
                echo "linting"
            }
        }
        stage('Functional test') {
            steps {
                echo "testing"
            }
        }
    }
}
