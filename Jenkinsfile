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
                    // Maven-Variablen korrekt escapen
                    sh '''
                        mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\${parsedVersion.majorVersion}.\\${parsedVersion.minorVersion}.\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit
                    '''

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
            }
        }

        stage('build image') {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-repo',
                        passwordVariable: 'PASS',
                        usernameVariable: 'USER'
                    )]) {
                        sh "docker build -t asdhka/annirep:${env.IMAGE_NAME} ."
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh "docker push asdhka/annirep:${env.IMAGE_NAME}"
                    }
                }
            }
        }

        stage('deploy') {
            steps {
                script {
                    echo 'deploying docker image...'
                }
            }
        }

        stage('commit version update') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'github-jenkins-token',
                        passwordVariable: 'PASS',
                        usernameVariable: 'USER'
                    )]) {

                        // Git User konfigurieren
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        // Branch wechseln & Remote Änderungen einholen
                        sh '''
                            git checkout jenkins-jobs
                            git fetch origin jenkins-jobs
                            git rebase origin/jenkins-jobs
                        '''

                        // Remote URL auf Credential setzen
                        sh '''
                            git remote set-url origin https://${USER}:${PASS}@github.com/coffeezon3/java-maven-app-ci.git
                        '''

                        // Änderungen committen, nur wenn pom.xml geändert wurde
                        sh '''
                            git add pom.xml
                            git diff --cached --quiet || git commit -m "ci: version bump"
                        '''

                        // Änderungen pushen
                        sh 'git push origin jenkins-jobs'
                    }
                }
            }
        }

    }
}
