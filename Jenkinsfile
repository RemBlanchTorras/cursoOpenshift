pipeline {
  agent any
  stages {
    stage('Cleanup Workspace') {
      steps {
        cleanWs()
        sh '''
                echo "Cleaned Up Workspace For Project"
                '''
      }
    }

    stage('Code Checkout') {
      steps {
        checkout([
                                                  $class: 'GitSCM', 
                                                  branches: [[name: '*/main']], 
                                                  userRemoteConfigs: [[url: 'https://github.com/spring-projects/spring-petclinic.git']]
                                              ])
          slackSend(channel: 'jenkins-notifies', color: 'good', message: "Build Started - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)")
        }
      }

      stage(' Unit Testing') {
        steps {
          sh '''
                echo "Running Unit Tests"
                '''
        }
      }

      stage('Code Analysis') {
        steps {
          sh '''
                echo "Running Code Analysis"
                '''
        }
      }

      stage('Build Deploy Code') {
        when {
          branch 'develop'
        }
        steps {
          sh '''
                echo "Building Artifact"
                '''
          sh '''
                echo "Deploying Code"
                '''
          slackSend(color: '#439FE0', message: "Build deployed successfully - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)")
        }
      }

    }
    options {
      buildDiscarder(logRotator(daysToKeepStr: '16', numToKeepStr: '10'))
    }
  }
