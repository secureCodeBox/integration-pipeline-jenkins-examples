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
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/develop/cli/run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/develop/cli/sslyze.template.json'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/develop/cli/nmap.template.json'
        fileExists 'run_scanner.sh'
        sh 'chmod +x run_scanner.sh'
      }
    }
    stage('Execute SCB Tests') {
      steps {
        parallel(
          "Run Nmap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -a "`echo -n $ENGINE_CREDS | base64`" -i 500 -w 2 nmap $TARGET_HOST'
            archiveArtifacts 'job__nmap_result.json,job__nmap_result.readable.txt'

          },
          "Run SSLyze Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -a "`echo -n $ENGINE_CREDS | base64`" -i 500 -w 2 sslyze $TARGET_HOST'
            archiveArtifacts 'job__sslyze_result.json,job__sslyze_result.readable.txt'

          }
        )
      }
    }
  }
  environment {
    TARGET_HOST = 'www.secureCodeBox.io'
    TARGET_URL = 'https://www.secureCodeBox.io'
    ENGINE_URL = 'http://engine.secure-code-box.svc:8080'
    ELASTIC_URL = 'http://elasticsearch.secure-code-box.svc:9200'
    ENGINE_CREDS = credentials('scb-internal-dev-scanner-jenkins')
  }
}
