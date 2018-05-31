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
      agent {
        docker {
          image 'buildo/alpine-ssh'
        }
      }
      steps {
        unstash 'add2vals'
        echo 'Upload to bower-test'
        sh 'ls -lR'
        sh 'scp dist/add2vals pi@bower:/home/pi/add2vals'
      }
    }
    stage('Deploy for production') {
      when {
        branch 'production'
      }
      steps {
        echo 'Upload to bower'
        echo 'Run on bower'
        sh 'ls'
      }
    }
  }
  environment {
    CI = 'true'
  }
}
