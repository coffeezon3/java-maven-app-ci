#!/usr/bin/env groovy
@Library('jenkins-shared-library') _
def gv

pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }

    stages {
        stage("init") {
            steps {
                script {
                    // Lädt die globalen Funktionen aus script.groovy
                    gv = load('script.groovy')
                }
            }
        }

        stage("build jar") {
            steps {
                script {
                    // Baut das Maven-Projekt
                    buildJar()
                }
            }
        }

        stage("build image") {
            steps {
                script {
                    // Docker-Objekt erzeugen und Image bauen/pushen
                    def docker = new com.example.Docker(this)
                    docker.buildDockerImage('asdhka/annirep:jma-3.0')
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    // Deployment-Methode aus der Shared Library aufrufen
                    gv.deployApp()
                }
            }
        }
    }
}
