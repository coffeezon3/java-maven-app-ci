pipeline {
    agent any

    parameters {
        choice(
            name: 'VERSION',
            choices: ['1.1.0', '1.2.0', '1.3.0'],
            description: 'Select version to deploy'
        )
        booleanParam(
            name: 'executeTests',
            defaultValue: true,
            description: 'Run tests or not'
        )
    }

    tools {
        maven 'maven-3.9'
    }

    stages {

        stage('Build') {
            steps {
                echo 'building the application'
                echo "building version ${params.VERSION}"
            }
        }

        stage('Test') {
            when {
                expression { params.executeTests }
            }
            steps {
                echo 'testing the application'
            }
        }

        stage('Deploy') {
            steps {
                echo 'deploying the application'
                echo "deploying version ${params.VERSION}"
            }
        }
    }
}
