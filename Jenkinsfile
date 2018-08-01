pipeline {
  agent any
  stages {
    stage('Download CLI') {
      steps {
        sh 'rm run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/cli-custom-payload/cli/run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/sslyze.template.json'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/nmap.template.json'
        fileExists 'run_scanner.sh'
        sh 'chmod +x run_scanner.sh'
      }
    }
    stage('Nmap Scan') {
      steps {
        parallel(
          "Nmap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -i 500 -w 2 nmap http://localhost'
            archiveArtifacts 'job__nmap_result.json,job__nmap_result.readable'
            
          },
          "SSLyze Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -i 500 -w 2 sslyze https://iteratec.de:443'
            archiveArtifacts 'job__sslyze_result.json,job__sslyze_result.readable'
            
          },
          "Custom Arachni Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -i 50000 -w 2 -p arachni-scan-long.json arachni'
            archiveArtifacts 'job__arachni_result.json,job__arachni_result.readable'
            
          },
          "Custom Zap Scan": {
            sh './run_scanner.sh -b $ENGINE_URL $ELASTIC_URL -i 50000 -w 2 -p zap-scan-long.json zap'
            archiveArtifacts 'job__zap_result.json,job__zap_result.readable'
            
          }
        )
      }
    }
  }
  environment {
    ENGINE_URL = 'http://engine-oss.secure-code-box-jannik-ansible.svc:8080'
    ELASTIC_URL = 'http://elasticsearch.secure-code-box-jannik-ansible.svc:9200'
  }
}