
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
            Ping()
            }
        }
    }
    post {
        always {
            slackSend  message: "Pipeline $JOB_NAME built $BRANCH_NAME build #$BUILD_NUMBER at node $NODE_NAME with status: ${currentBuild.currentResult}!"
            Archive()
        }
    }
}

