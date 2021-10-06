
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
            Main.install()
        }
        }
        stage('Run test') {
        steps {
            Main.test()
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
    sh('git tag -a "${BUILD_NUMBER}" -m "Added build tag"')
    sh('git push --tags')
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
            Ping()
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

