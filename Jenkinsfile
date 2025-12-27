pipeline {
    agent any

    tools {
        maven 'maven-3.9'
    }

    stages {
        stage('Build') {
            steps {
                echo 'building the application'
            }
        }

        stage('test') {
            steps {
                echo 'testing the application'
            }
        }
        
        stage('deploy') {
            steps {
                echo 'deploying the application'
            }
        }
    }
}
