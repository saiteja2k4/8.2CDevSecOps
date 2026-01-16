pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/saiteja2k4/8.2CDevSecOps.git', branch: 'main'

      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        // don’t fail the whole build if snyk isn’t authenticated
        bat 'npm test || exit /b 0'
      }
    }

    stage('Generate Coverage Report') {
      steps {
        // nodejs-goof may not have coverage script; don’t fail build
        bat 'npm run coverage || exit /b 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        bat 'npm audit || exit /b 0'
      }
    }
  }
}
post {
  always {
    emailext(
      subject: "Jenkins Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
      body: """Hi,

Build Status: ${currentBuild.currentResult}
Job Name: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}

Regards,
Jenkins
""",
      to: "saiteja07042004@gmail.com",
      attachLog: true
    )
  }
}
