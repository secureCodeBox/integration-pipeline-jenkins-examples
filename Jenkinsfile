pipeline {
  agent any
  triggers {
        cron('H */12 * * *')
  }
  options {
      timeout(time: 2, unit: 'MINUTES')
  }
  stages {
    stage('Initilize SCB CLI') {
      steps {
        sh 'rm -f run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/c8d7cfcfeb16bbe8ff3592c3492b8eb6510591a9/cli/run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/c8d7cfcfeb16bbe8ff3592c3492b8eb6510591a9/cli/sslyze.template.json'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/c8d7cfcfeb16bbe8ff3592c3492b8eb6510591a9/cli/nmap.template.json'
        fileExists 'run_scanner.sh'
        sh 'chmod +x run_scanner.sh'
      }
    }
    stage('Execute SCB Tests') {
      steps {
        parallel(
          "Run Nmap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL -a $ENGINE_CREDS -i 120 -w 1 -n $TARGET_NAME -c $TARGET_NAME nmap $TARGET_HOST'
            archiveArtifacts 'job_nmap_result.json'

          },
          "Run SSLyze Scan": {
            sh './run_scanner.sh -b $ENGINE_URL -a $ENGINE_CREDS -i 120 -w 1 -n $TARGET_NAME -c $TARGET_NAME sslyze $TARGET_HOST'
            archiveArtifacts 'job_sslyze_result.json'

          }
        )
      }
    }
  }
  environment {
    TARGET_HOST = 'www.secureCodeBox.io'
    TARGET_NAME = 'securecodebox.io'
    TARGET_URL = 'https://www.secureCodeBox.io'
    ENGINE_URL = 'http://engine.secure-code-box.svc:8080'
    ENGINE_CREDS = credentials('scb-internal-dev-scanner-jenkins')
  }
}
