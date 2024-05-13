@Library('shared-library') _

pipeline {
  agent {
    kubernetes {
      inheritFrom 'maven-pvc'
    }
  }
environment {
   BRANCH = "${GIT_BRANCH.split("/")[1]}"
   TAG = "${BRANCH}-${BUILD_TIMESTAMP}"
}
  stages {
    stage('Build') {
      steps {
        gitBranch()
      }
    }
    stage('SonarQube Check') {
      steps {
        container('maven-pvc') {
        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'sonarqube') {
        sonarqubeCheck()
          }
          def qualitygate = waitForQualityGate()
          if (qualitygate.status != "OK") {
             error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
          }
        }
      }
    }
    stage("Quality gate") {
      steps {
        echo "Hello"
      }
    }
  }
}
