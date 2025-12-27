pipeline {
    agent {
        docker {
            image 'maven:3.9.0-openjdk-8'
            args '-u root:root' // optional, falls Schreibrechte nötig
        }
    }

    environment {
        DOCKER_IMAGE = 'asdhka/annirep'
    }

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

        stage('Increment Version') {
            steps {
                script {
                    echo 'Incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set -DnextSnapshot=false versions:commit'
                    // Lesen der neuen Version aus pom.xml
                    def version = sh(script: "mvn help:evaluate -Dexpression=project.version -q -DforceStdout", returnStdout: true).trim()
                    env.IMAGE_TAG = "${version}-${BUILD_NUMBER}"
                    echo "IMAGE_TAG set to ${env.IMAGE_TAG}"
                }
            }
        }

        stage('Build App') {
            steps {
                script {
                    echo 'Building the application...'
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    script {
                        echo 'Building Docker image...'
                        sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
                        echo 'Logging in to Docker Hub...'
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        echo 'Pushing Docker image...'
                        sh "docker push ${DOCKER_IMAGE}:${IMAGE_TAG}"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying Docker image ${DOCKER_IMAGE}:${IMAGE_TAG}..."
                    // Optional: Deployment Script hier
                }
            }
        }

        stage('Commit Version Update') {
            steps {
                withCreden

