pipeline {
    agent any

    tools {
        maven 'maven-3.9'  // exakter Name aus Jenkins
        jdk 'jdk-17'       // exakter Name aus Jenkins
    }

    stages {
        stage('Clean Workspace') {
            steps { deleteDir() }
        }

        stage('Checkout SCM') {
            steps { checkout scm }
        }

        stage('Increment Version') {
            steps {
                script {
                    echo 'Incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set -DnextSnapshot=false versions:commit'
                    env.IMAGE_TAG = sh(script: "mvn help:evaluate -Dexpression=project.version -q -DforceStdout", returnStdout: true).trim() + "-${BUILD_NUMBER}"
                    echo "IMAGE_TAG set to ${env.IMAGE_TAG}"
                }
            }
        }

        stage('Build App') {
            steps { sh 'mvn clean package' }
        }

        stage('Build & Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    docker build -t asdhka/annirep:${IMAGE_TAG} .
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push asdhka/annirep:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Deploy') {
            steps { echo "Deploying Docker image ${IMAGE_TAG}..." }
        }

        stage('Commit Version Update') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    git config user.email "jenkins@example.com"
                    git config user.name "jenkins"
                    git add pom.xml
                    git commit -m "Update app version to ${IMAGE_TAG}"
                    git push https://$USER:$PASS@github.com/coffeezon3/java-maven-app-ci.git HEAD:jenkins-jobs
                    """
                }
            }
        }
    }
}
