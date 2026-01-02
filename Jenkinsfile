#!/usr/bin/env groovy
@Library('jenkins-shared-library') _
def gv

pipeline {
    agent any
    tools { maven 'maven-3.9' }

    stages {
        stage("init") {
            steps {
                script { gv = load('script.groovy') }
            }
        }

        stage("build jar") {
            steps { script { buildJar() } }
        }

        stage("build image") {
            steps {
                script {
                    def docker = new com.example.Docker(this)
                    docker.buildDockerImage('asdhka/annirep:jma-3.0')
                }
            }
        }

        stage("deploy") {
            steps { script { gv.deployApp() } }
        }
    }
}
