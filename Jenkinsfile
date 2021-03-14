pipeline {
    agent builder
        stages {
            stage("Install modules") {
                steps {
                    sh """
                    npm install
                    """
                }
            }
            stage("Production build") {
                when {
                    branch "master"
                }
                steps {
                    sh """
                    npm run build
                    """
                }
            }
            stage("Prod publish over SSH") {
                when {
                    branch "master"
                }
                steps {
                    sshPublisher(publishers: [sshPublisherDesc(configName: 'webserver-prod', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                }
            }
    }
}