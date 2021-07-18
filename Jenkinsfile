pipeline {
  agent any
  stages {
    stage ("Build") {
      steps {
        sh './gradlew build --no-daemon'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
      }
    }  
    stage("Deploy to stage"){
      steps {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'staging', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/tmp', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/trainSchedule.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }
    stage("Deploy to prod"){
      steps {
        input 'Does the staging environment look OK?'
        milestone(1)
        sshPublisher(publishers: [sshPublisherDesc(configName: 'staging', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/tmp', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/trainSchedule.zip')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }
  }  
} 
   
