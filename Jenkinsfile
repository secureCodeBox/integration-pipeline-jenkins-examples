pipeline {
  agent any
  stages {
    stage('Nmap Scan') {
      parallel {
        stage('Start Nmap Scan') {
          steps {
            sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/run_scanner.sh'
            sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/nmap.template.json'
            fileExists 'run_scanner.sh'
            sh 'chmod +x run_scanner.sh'
            sh './run_scanner.sh -b http://engine-oss.secure-code-box-jannik-ansible.svc:8080 http://elasticsearch.secure-code-box-jannik-ansible.svc:9200 -i 500 -w 2 nmap http://localhost'
            sh 'cat job__nmap_result.readable'
            archiveArtifacts 'job__nmap_result.json,job__nmap_result.readable'
          }
        }
        stage('SSLyze Scan') {
          steps {
            echo 'Hello World'
          }
        }
      }
    }
  }
}