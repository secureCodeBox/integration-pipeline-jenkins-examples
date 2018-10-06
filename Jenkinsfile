pipeline {
  agent any
  triggers {
        cron('H */12 * * *')
  }
  stages {
    stage('Initilize SCB CLI') {
      steps {
        sh 'rm -f run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/cli-custom-payload/cli/run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/sslyze.template.json'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/nmap.template.json'
        fileExists 'run_scanner.sh'
        sh 'chmod +x run_scanner.sh'
      }
    }
    stage('Execute SCB Tests') {
      steps {
        parallel(
          "Run Nmap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -i 500 -w 2 nmap $TARGET_HOST'
            archiveArtifacts 'job__nmap_result.json,job__nmap_result.readable.txt'

          }
        )
      }
    }
  }
  environment {
    TARGET_HOST = 'secureCodeBox.io'
    TARGET_URL = 'https://www.secureCodeBox.io'
    ENGINE_URL = 'http://engine-oss.secure-code-box.svc:8080'
    ELASTIC_URL = 'http://elasticsearch.secure-code-box.svc:9200'
  }
}
