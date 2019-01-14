pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'hello wold'
        bat 'gradle build '
        bat 'gradle jar '
        bat 'gradle javadoc '
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
            withSonarQubeEnv('sonarqube') {
              bat 'sonar-scanner'
            }

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
        bat 'gradle uploadArchives'
      }
    }
    stage('slack notification') {
      steps {
        slackSend(channel: '#general', message: 'build and deployment done')
      }
    }
  }
  tools {
    gradle 'gradle'
  }
}