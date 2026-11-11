pipeline {
  agent any
  environment {
    WEBEX_TOKEN = credentials('webex-token')
    WEBEX_ROOM  = credentials('webex-room')
  }
  stages {
    stage('Test')  {
      steps {
        sh 'python3 -m unittest -v'
      }
    }
  }
  post  {
    always  {
      script  { env.RESULT = currentBUILD.currentResult }
      sh '''
        curl -s -X POST https://webexapis.com/v1/messages \\
          -H "Authorization: Bearer $WEBEX_TOKEN" \\
          -H "Content-Type: application/json" \\
          -d "{\\"roomId\\": \\"$WEBEX_ROOM\\", \\"text\\": \\"Build $JOB_NAME #$BUILD_NUMBER: $RESULT - $BUILD_URL\\"}"
      '''
    }
  }
}