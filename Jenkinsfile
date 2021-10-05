pipeline {
    agent any
    environment {
        SKIP_COMMIT_MSG = 'SKIP_CI'
    }

    stages {
        stage('Install modules') {
        steps {
            sh 'npm install'
        }
        }
        stage('Run test') {
        steps {
            sh 'npm run test'
        }
        }
        stage('Get commit message and branch') {
        steps {
            script {
                env.GIT_BRANCH = $(git rev-parse --abbrev-ref HEAD)
                env.GIT_COMMIT_MSG = $(git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim())
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
            sh 'npm run build'
            }
        }

        stage('Ping websites') {
        steps {
            parallel(
            a: {
                sh 'ping -c 3 instagram.com'
            },
            b: {
                sh 'ping -c 3 vk.com'
            },
            c: {
                sh 'ping -c 3 facebook.com'
            }
            )
        }
        }
    }
    post {
         always {
             zip artifact-${GIT_BRANCH}-${BUILD_NUMBER}.zip ./dist/
            //##  archiveArtifacts artifacts: '**/dist/', followSymlinks: false, allowEmptyArchive: true
         }
     }
}

