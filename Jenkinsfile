// Only keep the 10 most recent builds
properties([[$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', numToKeepStr: '10']]])

pipeline {
    agent any
    post {
      failure {
        updateGitlabCommitStatus name: 'build', state: 'failed'
      }
      success {
        updateGitlabCommitStatus name: 'build', state: 'success'
      }
    }
    triggers {
        gitlab(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
    }
environment {
Resource_FILE_GIT="xxxxxxxxxxxxxxxxxxxxxxx"
CF_API="xxxxxxxxxxxxx"
CF_ORG="xxxxxxxxx"
CF_SPACE="xxxxxxxxxx"
CF_USERNAME="xxxxxxxxxxxx"
CF_PASSWORD="xxx"
BUILD_HOME='/var/lib/jenkins/workspace'
}
stages {
  stage('Copy data files to local') {
      steps { 
         sh "rm -rf arc001-*"
         sh "git clone $Resource_FILE_GIT"
        }
    }

  stage('Push ruby backend application') {
       steps {
         sh '''cf login -a $CF_API --skip-ssl-validation -u $CF_USERNAME -p $CF_PASSWORD
		 cf target -o $CF_ORG -s $CF_SPACE
		 cf push'''
        }
    }
}
}
