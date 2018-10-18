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
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/cli-custom-payload/cli/run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/sslyze.template.json'
        fileExists 'run_scanner.sh'
        sh 'chmod +x run_scanner.sh'
      }
    }
    stage('Execute SCB Tests') {
      steps {
        parallel(
          "Run Nmap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -i 500 -w 2 -p nmap-scan.json nmap'
            archiveArtifacts 'job__nmap_result.json,job__nmap_result.readable'

          },
          "Run Arachni Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -i 50000 -w 2 -p arachni-scan-long.json arachni'
            archiveArtifacts 'job__arachni_result.json,job__arachni_result.readable'

          },
          "Run Zap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -i 50000 -w 2 -p zap-scan-quick.json zap'
            archiveArtifacts 'job__zap_result.json,job__zap_result.readable'

          }
        )
      }
    }
  }
  environment {
    TARGET_HOST = 'juice-shop.secure-code-box.svc'
    TARGET_URL = 'http://juice-shop.secure-code-box.svc:3000'
    ENGINE_URL = 'http://engine-oss.secure-code-box.svc:8080'
    ELASTIC_URL = 'http://elasticsearch.secure-code-box.svc:9200'
  }
}
