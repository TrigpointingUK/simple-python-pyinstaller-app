pipeline {
  agent none
  stages {
    stage('Build') {
      agent {
        docker {
          image 'python:2-alpine'
        }

      }
      steps {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      }
    }
    stage('Test') {
      agent {
        docker {
          image 'qnib/pytest'
        }
      }
      steps {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
      post {
        always {
          junit 'test-reports/results.xml'
        }
      }
    }
    stage('Deliver') {
      agent {
        docker {
          image 'cdrx/pyinstaller-linux:python2'
        }
      }
      steps {
        sh 'pyinstaller --onefile sources/add2vals.py'
      }
      post {
        success {
          archiveArtifacts 'dist/add2vals'
          stash includes: 'dist/add2vals', name: 'add2vals'
        }
      }
    }
    stage('Deploy to development') {
      when {
        branch 'development'
      }
      agent any
      steps {
        unstash 'add2vals'
        echo 'Upload to bower-test'
        sh 'ls -lR'
        sshPublisher(publishers: [sshPublisherDesc(configName: 'bower-test', transfers: [sshTransfer(excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'dist/', sourceFiles: 'dist/add2vals')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
      }
    }
    stage('Deploy for production') {
      when {
        branch 'production'
      }
      steps {
        echo 'Upload to bower'
      }
    }
  }
  environment {
    CI = 'true'
  }
}
