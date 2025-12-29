pipeline {
    agent any

    tools {
        maven 'maven-3.9'
    }

    stages {

        stage('Info') {
            steps {
                echo "Branch: ${env.BRANCH_NAME}"
                echo "Build number: ${env.BUILD_NUMBER}"
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            when {
                branch 'master'
            }
            steps {
                echo 'Running tests on master branch'
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            when {
                branch 'jenkins-jobs'
            }
            steps {
                echo 'Deploying from jenkins-jobs branch'
            }
        }
    }
}
