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
    }
}