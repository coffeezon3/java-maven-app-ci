pipeline {
    agent any

    stages {
        stage('Hello TWN') {
            steps {
                echo '🚀 Jenkins Pipeline läuft!'
                echo "Branch: ${env.BRANCH_NAME}"
                echo "Build Nummer: ${env.BUILD_NUMBER}"
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
