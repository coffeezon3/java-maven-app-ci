pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                script {
                    echo 'Cleaning workspace...'
                    deleteDir() // löscht alles im Workspace
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Tool Install') {
            steps {
                // Dein Tool Install Code hier
            }
        }

        stage('Increment Version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set -DnextSnapshot=false versions:commit'
                    // set IMAGE_NAME etc.
                }
            }
        }

        stage('Build App') {
            steps {
                script {
                    echo 'building the application...'
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    echo 'building the docker image...'
                    // Docker Build & Push
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'deploying docker image...'
                }
            }
        }

        stage('Commit Version Update') {
            steps {
                script {
                    // Dein git commit Schritt
                }
            }
        }
    }
}
