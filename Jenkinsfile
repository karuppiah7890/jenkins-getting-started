pipeline {
    agent any

    stages {
        stage('environment-parameters') {
            steps {
                script {
                    properties([
                        parameters([
                            choice(
                                name: 'ENVIRONMENT',
                                choices: ['DEV', 'QA'],
                                description: 'Choose an environment'
                            )
                        ]
                    )])
                }
            }
        }
        stage('Hello') {
            steps {
                echo "Hello World ${params.ENVIRONMENT}"
            }
        }
        stage('tag-only') {
            when { buildingTag() }
            steps {
                script {
                    sh "$TAG_NAME"
                    sh "$GIT_COMMIT"
                }
            }
        }
    }
}

