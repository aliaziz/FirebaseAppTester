node {

    def isMainline = ["develop", "master"].contains(env.BRANCH_NAME)

    List environment = [
        "GOOGLE_APPLICATION_CREDENTIALS=$HOME/.jenkins/workspace/DistroBranchable_master/service_creds_app_distro.json"
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

    stage 'Build Debug'
        sh "./gradlew assembleDebug"


   if (isMainline) {

        stage 'Archive'
             archiveArtifacts artifacts: 'app/build/outputs/apk/debug/*.apk', fingerprint: false, allowEmptyArchive: false

          stage ('Distribute') {
              withEnv(environment) {
                  sh "./gradlew assembleDebug appDistributionUploadDebug"
              }
          }

    }

}
