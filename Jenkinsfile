pipeline {
    agent any

    tools {
        maven 'maven-3.9'
    }

    environment {
        // Globale Variable für das Image
        IMAGE_NAME = ''
    }

    stages {

        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    // Maven-Variablen mit escaped $ damit Jenkins sie nicht interpretiert
                    sh '''
                        mvn build-helper:parse-version versions:set \
                            -DnewVersion=\\${parsedVersion.majorVersion}.\\${parsedVersion.minorVersion}.\\${parsedVersion.nextIncrementalVersion} \
                            versions:commit
                    '''
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
            }
        }

        stage('build image') {
            steps {
                script {
                    echo 'building the docker image...'
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-repo',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )]) {
                        sh "docker build -t asdhka/annirep:${IMAGE_NAME} ."
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh "docker push asdhka/annirep:${IMAGE_NAME}"
                    }
                }
            }
        }

        stage('deploy') {
            steps {
                echo 'deploying docker image...'
            }
        }

        stage('commit version update') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'github-jenkins-token',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )]) {

                        // Git Config
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        // Auf den Branch wechseln
                        sh 'git checkout jenkins-jobs'

                        // Remote URL setzen mit Credentials
                        sh '''
                            git remote set-url origin https://${USER}:${PASS}@github.com/coffeezon3/java-maven-app-ci.git
                        '''

                        // Nur pom.xml committen
                        sh 'git add pom.xml'
                        sh 'git diff --cached --quiet || git commit -m "ci: version bump"'
                        sh 'git push origin jenkins-jobs'
                    }
                }
            }
        }

    }
}

