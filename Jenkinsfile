def VARIABLE = 'VARIABLE'
string ANOTHER_VARIABLE = 'ANOTHER_VARIABLE'
YET_ANOTHER_VARIABLE = 'YET_ANOTHER_VARIABLE'

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
        stage('creating-dummy-artifact-file') {
            steps {
                script {
                    sh "echo 'sample-file-content' > dummy-file.txt"
                }
            }
        }
        stage('accessing-dummy-artifact-file-in-another-stage') {
            steps {
                script {
                    sh 'cat dummy-file.txt'
                }
            }
        }
        stage('accessing-variables') {
            steps {
                script {
                    echo "${VARIABLE}"
                    echo "${ANOTHER_VARIABLE}"
                    echo "${YET_ANOTHER_VARIABLE}"
                    echo YET_ANOTHER_VARIABLE
                    PREVIOUS_STAGE_VARIABLE = env.GIT_COMMIT
                }
            }
        }
        stage('approve-to-proceed-to-the-next-step') {
            options {
                timeout(time: 3, unit: 'SECONDS')
            }
            steps {
                script {
                    properties([
                        parameters([
                            choice(
                                name: 'DO_ACTION',
                                choices: ['NO', 'YES'],
                                description: 'Do the next action?'
                            )
                        ]
                    )])
                }
            }
        }
        stage('some-action') {
            when {
                expression {
                    return params.ENVIRONMENT == "YES"
                }
            }
            steps {
                script {
                    echo "Doing some action! I remember the variable too! It's ${PREVIOUS_STAGE_VARIABLE}"
                }
            }
        }
    }
}
