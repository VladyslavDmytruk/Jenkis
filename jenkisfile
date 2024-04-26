pipeline {
    agent any

    stages {
        stage('Install Apache2') {
            steps {
                sh 'sudo apt-get update && sudo apt-get install -y apache2'
            }
        }
        stage('Check Apache Logs') {
            steps {
                script {
                    def log = sh(script: 'sudo cat /var/log/apache2/access.log', returnStdout: true).trim()
                    def errorCount = 0

                    log.eachLine { line ->
                        if (line =~ /.*\s(4\d{2}|5\d{2})\s.*/) {
                            errorCount++
                        }
                    }

                    if (errorCount > 0) {
                        error "Found ${errorCount} errors in Apache access log"
                    }
                }
            }
        }
    }
}
