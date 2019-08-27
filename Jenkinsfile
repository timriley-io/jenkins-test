#!/bin/groovy
pipeline {
  agent any
  tools {
    nodejs 'nodejs'
  }
  stages {
    stage('Startup') {
      steps {
        script {
          sh 'npm install'
          sh 'cd tests && npm install'
          // cypress deps
          //sh 'apt-get install -y xvfb libgtk-3-dev libnotify-dev libgconf-2-4 libnss3 libxss1 libasound2'
        }
      }
    }
    stage('Test') {
      steps {
        script {
          sh 'npm run test:ci'
        }
      }
      post {
        always {
          step([$class: 'CoberturaPublisher', coberturaReportFile: 'output/coverage/jest/cobertura-coverage.xml'])
        }
      }
    }
    stage('Build') {
      steps {
        script {
          sh 'npm run build'
          sh 'npm pack'
        }
      }
    }
    /*stage('Deploy') {
      when {
        expression {
          currentBuild.result == null || currentBuild.result == 'SUCCESS'
        }
      }
      steps {
        script {
          def server = Artifactory.server 'My_Artifactory'
          uploadArtifact(server)
        }
      }
    }*/
  }
}

/*def uploadArtifact(server) {
  def uploadSpec = """{
            "files": [
              {
                "pattern": "continuous-test-code-coverage-guide*.tgz",
                "target": "npm-stable/"
              }
           ]
          }"""
  server.upload(uploadSpec)

  def buildInfo = Artifactory.newBuildInfo()
  server.upload spec: uploadSpec, buildInfo: buildInfo
  server.publishBuildInfo buildInfo
}*/