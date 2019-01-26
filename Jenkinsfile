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
        stage('Deploy-Staging') {
            steps {
                echo 'Deploying application to staging server'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'staging', transfers: [sshTransfer(excludes: '', execCommand: '''systemctl stop train-schedule
unzip -o trainSchedule.zip
systemctl start train-schedule''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'opt/train-schedule', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'dist/trainSchedule.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

            }
        }
    }
}
