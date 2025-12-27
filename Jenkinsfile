pipeline {
    agent any

    tools {
        maven 'maven-3.9'
    }

    stages {

        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    
                    // Versions-Inkrement via Maven
                    sh 'mvn build-helper:parse-version versions:set -DnextSnapshot=false versions:commit'

                    // Version aus pom.xml auslesen
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "${version}-${BUILD_NUMBER}"
                    echo "IMAGE_NAME set to ${env.IMAGE_NAME}"
                }
            }
        }

        stage('build app') {
            steps {
                script {
                    echo 'building the application...'
                    sh 'mvn clean package'
                }
