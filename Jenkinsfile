pipeline {
    agent any

    tools {
        maven 'maven-3.9'
    }

    stages {
        stage('Hello TWN') {
            steps {
                echo 'Jenkins Pipeline läuft!'
                echo "Build Nummer: ${BUILD_NUMBER}"
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn --version'
                sh 'mvn clean package'
            }
        }
    }
}
