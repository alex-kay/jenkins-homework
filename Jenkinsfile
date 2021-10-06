
@Library('pipeline-library-demo') _
Echo 'alex'
// pipeline {
//     agent any
//     environment {
//         SKIP_COMMIT_MSG = 'SKIP_CI'
//     }

//     stages {
//         stage('Test'){
//             steps{
//                 echo 'Testing shred library call:'
            
                
//             }
            
//         }
//         stage('Install modules') {
//         steps {
//             sh 'npm install'
//         }
//         }
//         stage('Run test') {
//         steps {
//             sh 'npm run test'
//         }
//         }
//         stage('Get commit message and branch') {
//         steps {
//             script {
//                 env.GIT_BRANCH = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
//                 env.GIT_COMMIT_MSG = sh(script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim()
//             }
//         }
//         }
//         stage('Build') {
//         when {
//             expression {
//             env.GIT_COMMIT_MSG != env.SKIP_COMMIT_MSG
//             }
//         }
//         steps {
//             sh '''
//             npm run build
//             git tag build-${BUILD_NUMBER}
//             git push --tags
//             '''
//             }
//         }

//         stage('Ping websites') {
//         steps {
//             parallel(
//             a: {
//                 sh 'ping -c 3 instagram.com'
//             },
//             b: {
//                 sh 'ping -c 3 vk.com'
//             },
//             c: {
//                 sh 'ping -c 3 facebook.com'
//             }
//             )
//         }
//         }
//     }
//     post {
//         always {
//             sh 'zip branch-${GIT_BRANCH}-build-${BUILD_NUMBER}.zip ./dist/* '
//             archiveArtifacts artifacts: '*.zip', followSymlinks: false, allowEmptyArchive: true
//         }
//     }
// }

