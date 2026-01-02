#!/user/bin/env groovy
@Library('jenkins-shared-library') _


pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage("init") {
            steps {
                script {

                }
            }
        }
        stage("build jar") {
            steps {
                script {
                  buildJar()
                }
            }
        }

        stage("build image") {
            steps {
                script {
                  buildImage()
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}

