pipeline {
        environment {
      /*
       * Uses a Jenkins credential called "FOOCredentials" and creates environment variables:
       * "$FOO" will contain string "USR:PSW"
       * "$FOO_USR" will contain string for Username
       * "$FOO_PSW" will contain string for Password
       */
      FOO = credentials("FOOcredentials")
    }
    agent any

    stages {
        stage('Checkout') {
            steps {
              git credentialsId: "${github_creds}", url: "${github_repo}"
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }

    post {

    // Email Ext plugin:
    success {

      emailext (
          subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
          to: "${emailRecipient}",
          from: "buildNotifications@emailaddress.com"
        )
    }

    failure {

      emailext (
          subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
          to: "${emailRecipient}",
          from: "buildNotifications@emailaddress.com"
        )
    }
  }
}
