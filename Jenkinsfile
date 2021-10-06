
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
            withCredentials([usernamePassword(credentialsId: 'github_key', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
    sh("git tag -a some_tag -m 'Jenkins'")
    sh('git push https://${GIT_USERNAME}:${GIT_PASSWORD}@${REPO_URL} --tags')
}
            sh '''
            npm run build
            git tag build-${BUILD_NUMBER}
            git push --tags
            '''
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
            sh 'zip branch-${GIT_BRANCH}-build-${BUILD_NUMBER}.zip ./dist/* '
            archiveArtifacts artifacts: '*.zip', followSymlinks: false, allowEmptyArchive: true
        }
    }
}

