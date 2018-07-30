pipeline {
  agent any
  stages {
    stage('Start Nmap Scan') {
      steps {
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/run_scanner.sh'
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/nmap.template.json'
        fileExists 'run_scanner.sh'
        sh 'chmod +x run_scanner.sh'
        sh './run_scanner.sh -b https://engine-oss-scb.cloudapps.iterashift.de:443 https://elasticsearch-scb.cloudapps.iterashift.de:443 -i 500 -w 2 nmap http://iteratec.de'
      }
    }
  }
}