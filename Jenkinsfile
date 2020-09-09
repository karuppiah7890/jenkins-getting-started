def VARIABLE = 'VARIABLE'
string ANOTHER_VARIABLE = 'ANOTHER_VARIABLE'
YET_ANOTHER_VARIABLE = 'YET_ANOTHER_VARIABLE'

pipeline {
    agent any

    stages {
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
                timeout(time: 5, unit: 'SECONDS')
            }
            steps {
                script {
                    Exception caughtException = null

                    catchError(buildResult: 'SUCCESS', stageResult: 'ABORTED') { 
                        try {
                            env.MY_CUSTOM_VARIABLE_FOR_ENV = input message: "Should we continue?",
                                                                ok: "Yes, we should.",
                                                                parameters: [choice(name: 'ENVIRONMENT',
                                                                    choices: 'QA',
                                                                    description: 'Which environment?')]
                        } catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
                            error "Stage timed out" 
                        } catch (Throwable e) {
                            caughtException = e
                        }
                    }

                    if (caughtException) {
                        error caughtException.message
                    }
                }
            }
        }
        stage('print-environment') {
            steps {
                script {
                    echo env.MY_CUSTOM_VARIABLE_FOR_ENV
                }
            }
        }
        stage('some-action-in-qa') {
            when {
                expression {
                    return env.MY_CUSTOM_VARIABLE_FOR_ENV == "QA"
                }
            }
            steps {
                script {
                    echo 'Doing some action in QA!'
                    echo 'I remember the previous stage variable too!'
                    echo "It's ${GIT_COMMIT}!"
                }
            }
        }
    }
}
