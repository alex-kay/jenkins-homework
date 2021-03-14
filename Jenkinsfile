pipeline {
    agent any
        stages {
            stage("Install modules") {
                steps {
                    sh """
                    npm install
                    """
                }
            }
            stage("Run test") {
                steps {
                    sh """
                    npm run test
                    """
                }
            }
            stage("Build") {
                steps {
                    sh """
                    npm run build
                    """
                }
            }
            stage("Dev publish over SSH") {
                when {
                    branch "dev"
                }
                steps {
                    sshPublisher(publishers: [sshPublisherDesc(configName: 'webserver-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                }
            }
            stage("Prod publish over SSH") {
                when {
                    branch "prod"
                }
                steps {
                    sshPublisher(publishers: [sshPublisherDesc(configName: 'webserver-prod', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                }
            }
    }
}