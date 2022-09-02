pipeline {
  agent any
  stages {
    stage('Buzz Build') {
      steps {
        sh '''./jenkins/build.sh
echo "I am a ${BUZZ_NAME}"'''
        archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
      }
    }

    stage('Buzz Test') {
      steps {
        sh './jenkins/test-all.sh'
        junit(testResults: '**/surefire-reports/**/*.xml', skipPublishingChecks: true)
      }
    }

  }
  environment {
    BUZZ_NAME = 'Worker Bee'
  }
}