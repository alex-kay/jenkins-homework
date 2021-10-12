
@Library('jenkins-shared-lib')_

pipeline {
    agent any
    options {
        parallelsAlwaysFailFast()
    }
    environment {
        SKIP_COMMIT_MSG = 'SKIP_CI'
        GIT_COMMIT_MSG = sh(script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim()
    }

    stages {
        stage('Install modules') {
        steps {
            Install()
        }
        }
        stage('Run test') {
        steps {
            Test()
        }
        }
        stage('Build') {
        when {
            expression {
            env.GIT_COMMIT_MSG != env.SKIP_COMMIT_MSG
            }
        }
        steps {
            Build()
            }
        steps {
            Ping()
            }
        }
    }
    post {
        always {
            echo "Result ${currentBuild.result}"
            Archive()
        }
        success {
            slackSend color: "good", message: "Pipeline $JOB_NAME built $BRANCH_NAME build #$BUILD_NUMBER at node $NODE_NAME succesfully!"
        }
        unstable {
            slackSend color: "warning", message: "Pipeline $JOB_NAME built $BRANCH_NAME build #$BUILD_NUMBER at node $NODE_NAME unstable!"
        }
        failure {
            slackSend color: "danger", message: "Pipeline $JOB_NAME built $BRANCH_NAME build #$BUILD_NUMBER at node $NODE_NAME failed!"
        }
    }
}

