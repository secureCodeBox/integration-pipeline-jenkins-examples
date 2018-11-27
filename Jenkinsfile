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
            sh './run_scanner.sh -b $ENGINE_URL -i 500 -w 2 -a $ENGINE_CREDS -p nmap-scan.json'
            archiveArtifacts 'job_nmap_result.json'

          },
          "Run Nikto Scan": {
            sh './run_scanner.sh -b $ENGINE_URL -i 500 -w 2 -a $ENGINE_CREDS -p nikto-scan.json'
            archiveArtifacts 'job_nikto_result.json'

          },
          "Run Arachni Scan": {
            sh './run_scanner.sh -b $ENGINE_URL -i 50000 -w 2 -a $ENGINE_CREDS -p arachni-scan-quick.json'
            archiveArtifacts 'job_arachni_result.json'

          },
          "Run Zap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL -i 50000 -w 2 -a $ENGINE_CREDS -p zap-scan-long.json'
            archiveArtifacts 'job_zap_result.json'

          }
        )
      }
    }
  }
  environment {
    TARGET_HOST = 'bodgeit.secure-code-box.svc'
    ENGINE_URL = 'http://engine-oss.secure-code-box.svc:8080'
    ENGINE_CREDS = credentials('scb-internal-dev-scanner-jenkins')
  }
}
