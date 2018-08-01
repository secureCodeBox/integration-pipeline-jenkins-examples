pipeline {
  agent any
  stages {
    stage('Download CLI') {
      steps {
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/sslyze.template.json'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/nmap.template.json'
        fileExists 'run_scanner.sh'
        sh 'chmod +x run_scanner.sh'
      }
    }
  }
  environment {
    ENGINE_URL = 'http://engine-oss.secure-code-box-jannik-ansible.svc:8080'
    ELASTIC_URL = 'http://elasticsearch.secure-code-box-jannik-ansible.svc:9200'
  }
}