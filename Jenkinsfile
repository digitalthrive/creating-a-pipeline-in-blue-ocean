pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }
    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
        input 'Finished using the web site? (Click "Proceed" to... proceed)'
        sh './jenkins/scripts/kill.sh'
      }
    }
    stage('Post') {
      steps {
        archiveArtifacts 'build/**/*'
        slackSend(channel: '@clay', color: 'good', message: 'creating-a-pipeline-in-blue-ocean build finished', tokenCredentialId: 'slack-token', teamDomain: 'warren-douglas.slack.com')
      }
    }
  }
  environment {
    npm_config_cache = 'npm-cache'
    CI = 'true'
  }
}
