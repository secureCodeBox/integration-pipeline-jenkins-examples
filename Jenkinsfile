pipeline {
  agent any
  stages {
    stage('Start Nmap Scan') {
      steps {
        sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/run_scanner.sh'
        fileExists 'run_scanner.sh'
        sh 'chmod +x run_scanner.sh'
        sh './run_scanner.sh --backend https://engine-oss-scb.cloudapps.iterashift.de https://elasticsearch-scb.cloudapps.iterashift.de --max_iter 500 --wait 2 nmap iteratec.de'
      }
    }
  }
}