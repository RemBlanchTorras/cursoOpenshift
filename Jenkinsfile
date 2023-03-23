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
          slackSend(channel: 'jenkins-notifies', color: 'green', message: 'This is a test')
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
          slackSend()
        }
      }

    }
    options {
      buildDiscarder(logRotator(daysToKeepStr: '16', numToKeepStr: '10'))
    }
  }