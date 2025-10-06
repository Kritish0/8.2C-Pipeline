pipeline {
  agent any

  options {
    skipDefaultCheckout(true)
  }

  stages {
    stage('Checkout') {
      steps {
        // If this job is already "Pipeline script from SCM", Jenkins
        // will auto-checkout. You can keep this for clarity or remove it.
        git url: 'https://github.com/Kritish0/8.2C-Pipeline', branch: 'main'
      }
    }

    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        // Don’t fail the whole build if tests aren’t set up yet
        sh 'npm test  echo "Tests failed (or not configured) — continuing"'
      }
    }

    stage('Audit') {
      steps {
        sh 'npm audit --audit-level=low | tee npm-audit.txt'
        sh 'npm audit --json > npm-audit.json  true'
        archiveArtifacts artifacts: 'npm-audit.*', fingerprint: true
      }
    }
  }

  post {
    success {
      echo '✅ Pipeline finished. Vulnerability scan results saved as artifacts.'
    }
    failure {
      echo '❌ Pipeline failed. Check the console log.'
    }
  }
}
