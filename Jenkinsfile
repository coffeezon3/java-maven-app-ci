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
                // Optional: Java, Maven etc.
            }
        }

        stage('Increment Version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set -DnextSnapshot=false versions:commit'
                    // Optional: IMAGE_NAME setzen
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
                    sh '''
                        docker build -t asdhka/annirep:${IMAGE_NAME} .
                        echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin
                        docker push asdhka/annirep:${IMAGE_NAME}
                    '''
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
                    echo 'Committing version update...'
                    sh '''
                        git config --global user.email "jenkins@example.com"
                        git config --global user.name "jenkins"
                        git add pom.xml
                        git commit -m "Increment version by Jenkins"
                        git push origin jenkins-jobs
                    '''
                }
            }
        }
    }
}
