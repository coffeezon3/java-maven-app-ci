pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                echo 'Cleaning workspace...'
                deleteDir() // löscht alles im Workspace
            }
        }

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Tool Install') {
            steps {
                echo 'Skipping tool install (oder hier Tool installieren)'
                // Beispiel für Maven Tool:
                // tool name: 'Maven 3.9.0', type: 'maven'
            }
        }

        stage('Increment Version') {
            steps {
                script {
                    echo 'Incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set -DnextSnapshot=false versions:commit'
                    // IMAGE_NAME setzen, falls benötigt:
                    // env.IMAGE_NAME = "myapp-${newVersion}"
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

        stage('Build Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    sh '''
                    docker build -t asdhka/annirep:1.1.8-PLACEHOLDER .
                    echo "Logging into Docker..."
                    echo $PASS | docker login -u asdhka --password-stdin
                    docker push asdhka/annirep:1.1.8-PLACEHOLDER
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Docker image... (hier Deploy-Skript einfügen)'
            }
        }

        stage('Commit Version Update') {
            steps {
                script {
                    echo 'Committing version update to Git...'
                    sh '''
                    git config --global user.email "jenkins@example.com"
                    git config --global user.name "jenkins"
                    git add pom.xml
                    git commit -m "Update version after build"
                    git push origin jenkins-jobs
                    '''
                }
            }
        }
    }
}

