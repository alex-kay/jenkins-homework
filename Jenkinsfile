
@Library('pipeline-library-demo')_

pipeline {
    agent any
    environment {
        SKIP_COMMIT_MSG = 'SKIP_CI'

    }

    stages {
        stage('Install modules') {
        steps {
            // sh 'npm install'
            Install()
        }
        }
        stage('Run test') {
        steps {
            Test()
        }
        }
        stage('Get commit message and branch') {
        steps {
            script {
                env.GIT_BRANCH = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                env.GIT_COMMIT_MSG = sh(script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim()
            }
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
    }
}

