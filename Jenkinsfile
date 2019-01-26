pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging') {
            when {
                branch 'master'
            }

            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                  sshPublisher alwaysPublishFromMaster: true, failOnError: true, publishers: [sshPublisherDesc(configName: 'staging', transfers: [sshTransfer(excludes: '', execCommand: 'unzip -o /tmp/trainScheudle.zip -d /opt-train-schedule/', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'tmp/', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/trainSchedule.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)]
                }
            }
        }
    }
}
