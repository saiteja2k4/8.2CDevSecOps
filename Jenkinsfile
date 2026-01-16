node {
  def buildResult = "SUCCESS"

  try {
    stage('Checkout') {
      checkout scm
    }

    stage('Install Dependencies') {
      bat 'npm install'
    }

    stage('Run Tests') {
      // Don't fail build if snyk not authenticated
      bat 'npm test || exit /b 0'
    }

    stage('NPM Audit (Security Scan)') {
      // Don't fail build, but show vulnerabilities in console
      bat 'npm audit || exit /b 0'
    }

  } catch (err) {
    buildResult = "FAILURE"
    currentBuild.result = "FAILURE"
    throw err
  } finally {
    // EMAIL ALWAYS (success or failure)
    emailext(
      subject: "Jenkins Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
      body: """Hi,

Build Status: ${currentBuild.currentResult}
Job: ${env.JOB_NAME}
Build: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}

Regards,
Jenkins
""",
      to: "saiteja07042004@gmail.com",
      attachLog: true
    )
  }
}
