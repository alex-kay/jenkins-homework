
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
        // stage('Get commit message and branch') {
        // steps {
        //     script {
        //         // env.GIT_BRANCH = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
        //         env.GIT_COMMIT_MSG = sh(script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim()
        //     }
        // }
        // }
        stage('Build') {
        when {
            expression {
            env.GIT_COMMIT_MSG != env.SKIP_COMMIT_MSG
            }
        }
        steps {
            Build()
            }
        }

        stage('Ping websites') {
        steps {
            Ping()
        }
        }
    }
    post {
        always {
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

