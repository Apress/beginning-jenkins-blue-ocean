pipeline {
  agent any
  stages {
    stage('Build & Test') {
      agent {
        node {
          label 'docker'
        }

      }
      steps {
        sh 'mvn -Dmaven.test.failure.ignore clean package'
        stash(name: 'build-test-artifacts', includes: '**/target/surfire-reports/TEST-*.xml,target/*.jar')
      }
    }

    stage('Report & Publish') {
      agent {
        node {
          label 'docker'
        }

      }
      steps {
        unstash 'build-test-artifacts'
        junit '**/target/surfire-reports/TEST-*.xml'
        archiveArtifacts(onlyIfSuccessful: true, artifacts: 'target/*.jar')
      }
    }

  }
}