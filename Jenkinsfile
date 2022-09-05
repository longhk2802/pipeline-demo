pipeline {
  agent none
  stages {
    stage('Buzz Build') {
      parallel {
        stage('build java 7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            sh '''./jenkins/build.sh
echo "I am a ${BUZZ_NAME} ee"'''
            archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
            stash(name: 'Buzz Java 7', includes: 'target/**')
          }
        }

        stage('buid java 8 ') {
          agent {
            node {
              label 'java8'
            }

          }
          environment {
            BUZZ_NAME = 'Java 8 Bee'
          }
          steps {
            sh '''./jenkins/build.sh
echo "I am a ${BUZZ_NAME} ee"'''
            archiveArtifacts(artifacts: 'target/*.jar', fingerprint: true)
            stash(name: 'Buzz Java 8', includes: 'target/**')
          }
        }

      }
    }

    stage('Buzz Test') {
      parallel {
        stage('Test A7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Buzz Java 7'
            sh './jenkins/test-all.sh'
            junit(testResults: '**/surefire-reports/**/*.xml', skipPublishingChecks: true)
          }
        }

        stage('Test B7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Buzz Java 8'
            sh '''sleep 10
echo done.'''
          }
        }

        stage('Test A8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Buzz Java 8'
            sh './jenkins/test-all.sh'
            junit(testResults: '**/surefire-reports/**/*.xml', skipPublishingChecks: true)
          }
        }

        stage('Test B8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Buzz Java 8'
            sh '''sleep 10
echo done.'''
          }
        }

      }
    }

    stage('Confirm Deploy to Staging') {
      when {
        branch 'master'
      }
      steps {
        input(message: 'Deploy to staging?', ok: 'yes')
      }
    }

    stage('Deploy to Staging') {
      agent {
        node {
          label 'java8'
        }

      }
      when {
        branch 'master'
      }
      steps {
        unstash 'Buzz Java 8'
        sh './jenkins/deploy.sh staging'
      }
    }

  }
  environment {
    BUZZ_NAME = 'Worker Bee'
  }
}