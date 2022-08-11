node {

    def isMainline = ["develop", "master"].contains(env.BRANCH_NAME)

    List environment = [
        "GOOGLE_APPLICATION_CREDENTIALS=$HOME/.android/service_creds_app_distro.json"
    ]

    stage('Checkout') {
        // Pull the code from the repo
        echo "$HOME"
        checkout scm
    }

    stage('Clean') {
        sh "./gradlew clean"
    }

    stage('Unit Test') {
        sh "./gradlew test"
    }

    stage 'Build Release'
        sh "./gradlew assembleRelease"


   if (isMainline) {

        stage 'Archive'
             archiveArtifacts artifacts: 'app/build/outputs/apk/release/*.apk', fingerprint: false, allowEmptyArchive: false

          stage ('Distribute') {
              withEnv(environment) {
                  sh "./gradlew assembleRelease appDistributionUploadDebug"
              }
          }

    }

}
