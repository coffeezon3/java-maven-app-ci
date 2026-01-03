#!/user/bin/env groovy

library identifier: 'jenkins-shared-library@master',  retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/coffeezon3/jenkins-shared-library.git',
     credentialsId: 'github-jenkins-token']
)

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
                   gv = load('script.groovy')
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

        stage("build and push image") {
            steps {
                script {
                  buildImage 'asdhka/annirep:jma-3.0'
                  dockerLogin()
                  dockerPush 'asdhka/annirep:jma-3.0'
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
