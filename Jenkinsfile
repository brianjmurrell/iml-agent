pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Build & Test') {
      parallel {
        stage('Build') {
          agent {
            node {
              label 'el7'
            }

          }
          steps {
            timeout(time: 30, activity: true) {
              sh 'make rpms'
            }

            stash(name: 'RPMs', includes: '_topdir/RPMS/noarch/*.rpm')
          }
        }
        stage('Unit Test') {
          agent {
            node {
              label 'el7'
            }

          }
          steps {
            sh 'nosetests || true'
          }
        }
      }
    }
    stage('Artifacts') {
      agent {
        node {
          label 'master'
        }

      }
      steps {
        unstash 'RPMs'
        archiveArtifacts(artifacts: '_topdir/RPMS/noarch/*.rpm', fingerprint: true, onlyIfSuccessful: true)
      }
    }
  }
}