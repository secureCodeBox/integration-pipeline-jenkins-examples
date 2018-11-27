pipeline {
  agent any
  triggers {
        cron('H */12 * * *')
  }
  options {
      timeout(time: 5, unit: 'HOURS')
  }
  stages {
    stage('Initilize SCB CLI') {
      steps {
        sh 'rm -f run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/6e507db5d51a6fc1a82910b627c8738982344abe/cli/run_scanner.sh'
        fileExists 'run_scanner.sh'
        sh 'chmod +x run_scanner.sh'
      }
    }
    stage('Execute SCB Tests') {
      steps {
        parallel(
          "Run Nmap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL -i 500 -w 2 -a $ENGINE_CREDS -p nmap-scan.json nmap'
            archiveArtifacts 'job_nmap_result.json'

          },
          "Run Arachni Scan": {
            sh './run_scanner.sh -b $ENGINE_URL -i 50000 -w 2 -a $ENGINE_CREDS -p arachni-scan-long.json arachni'
            archiveArtifacts 'job_arachni_result.json'

          },
          "Run Zap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL -i 50000 -w 2 -a $ENGINE_CREDS -p zap-scan-quick.json zap'
            archiveArtifacts 'job_zap_result.json'

          }
        )
      }
    }
  }
  environment {
    TARGET_HOST = 'juice-shop.secure-code-box.svc'
    TARGET_URL = 'http://juice-shop.secure-code-box.svc:3000'
    ENGINE_URL = 'http://engine.secure-code-box.svc:8080'
    ENGINE_CREDS = credentials('scb-internal-dev-scanner-jenkins')
  }
}
