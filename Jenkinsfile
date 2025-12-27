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
                    sh '''
                        mvn build-helper:parse-version versions:set \
                        -DnewVersion=${parsedVersion.majorVersion}.${parsedVersion.minorVersion}.${parsedVersion.nextIncrementalVersion} \
                        versions:commit
                    '''
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "${version}-${BUILD_NUMBER}"
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
                        sh "docker build -t asdhka/annirep:${IMAGE_NAME} ."
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh "docker push asdhka/annirep:${IMAGE_NAME}"
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
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )]) {
                        // Git-Konfiguration
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        // Branch wechseln (keine detached HEAD)
                        sh 'git checkout jenkins-jobs'

                        // Remote URL mit Token setzen
                        sh """
                            git remote set-url origin https://${USER}:${PASS}@github.com/coffeezon3/java-maven-app-ci.git
                        """

                        // Nur pom.xml committen
                        sh 'git add pom.xml'
                        sh 'git diff --cached --quiet || git commit -m "ci: version bump"'

                        // Push zum Branch
                        sh 'git push origin jenkins-jobs'
                    }
                }
            }
        }
    }
}

