pipeline {
  agent none
  stages {
    stage('Buzz Build') {
      agent {
        node {
          label 'java7'
        }

      }
      steps {
        sh '''./jenkins/build.sh
echo "I am a ${BUZZ_NAME} ee"'''
        archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
      }
    }

    stage('Buzz Test') {
      parallel {
        stage('Buzz Test') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            sh './jenkins/test-all.sh'
            junit(testResults: '**/surefire-reports/**/*.xml', skipPublishingChecks: true)
          }
        }

        stage('Buzz Test 2') {
          agent {
            node {
              label 'java7'
            }

          }
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