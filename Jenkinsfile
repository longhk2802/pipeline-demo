pipeline {
  agent any
  stages {
    stage('Buzz Build') {
      steps {
        sh '''./jenkins/build.sh
echo "I am a ${BUZZ_NAME} ee"'''
        archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
      }
    }

    stage('Buzz Test') {
      parallel {
        stage('Buzz Test') {
          steps {
            sh './jenkins/test-all.sh'
            junit(testResults: '**/surefire-reports/**/*.xml', skipPublishingChecks: true)
          }
        }

        stage('Buzz Test 2') {
          steps {
            sh '''sleep 10
echo done.'''
          }
        }

      }
    }

  }
  environment {
    BUZZ_NAME = 'Worker Bee'
  }
}