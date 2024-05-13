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
        }
      }
    }
    stage("Quality gate") {
      steps {
        qualityGateCheck.groovy
      }
    }
  }
}
