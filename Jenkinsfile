pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'gradle build '
        sh 'gradle jar '
        sh 'gradle javadoc'
        archiveArtifacts(artifacts: 'build/libs/*.jar', onlyIfSuccessful: true)
      }
    }
    stage('Mail Notification') {
      steps {
        mail(subject: 'Build status', body: 'something is going on', cc: 'fr_drissi@esi.dz', from: 'Me')
      }
    }
    stage('code Analysis') {
      parallel {
        stage('code Analysis') {
          steps {
            waitForQualityGate true
          }
        }
        stage('test reporting') {
          steps {
            jacoco()
          }
        }
      }
    }
    stage('Deployment') {
      steps {
        sh 'gradle uploadArchives'
      }
    }
    stage('slack notification') {
      steps {
        slackSend(channel: '#general', message: 'build and deployment done')
      }
    }
  }
}