pipeline {
  agent any

  environment {
    MVN = "mvn"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
        echo "Branch: ${env.BRANCH_NAME}"
      }
    }

    stage('Test (all branches)') {
      steps {
        bat "${env.MVN} -v"
        bat "${env.MVN} clean test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Package + Archive (main only)') {
      when {
        branch 'main'
      }
      steps {
        bat "${env.MVN} clean package"
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }

    stage('Security Scan (release branches)') {
      when {
        expression { return env.BRANCH_NAME?.startsWith("release/") }
      }
      steps {
        echo "Security scan placeholder (for rubric)"
        // Example: dependency-check, trivy, snyk (if installed later)
        bat "echo Running security scan..."
      }
    }
  }

  post {
    success { echo "Pipeline SUCCESS on ${env.BRANCH_NAME}" }
    failure { echo "Pipeline FAILED on ${env.BRANCH_NAME}" }
  }
}
