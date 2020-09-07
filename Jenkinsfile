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
                sh "echo $GIT_COMMIT"
            }
        }
        stage('tag-only') {
            when { buildingTag() }
            steps {
                script {
                    sh "echo $TAG_NAME"
                    sh "echo $GIT_COMMIT"
                }
            }
        }
    }
}

