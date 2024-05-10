@library ('shared-library') _

pipeline {
  agent {
    label 'maven'
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
        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'sonarqube') {
        sonarqubeCheck()
        }
      }
    }
  }
}
