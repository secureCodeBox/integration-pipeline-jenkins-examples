pipeline {
  agent any
  stages {
    stage('Nmap Scan') {
      parallel {
        stage('Nmap Scan') {
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
            sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/run_scanner.sh'
            sh 'wget https://raw.githubusercontent.com/secureCodeBox/secureCodeBox/master/cli/sslyze.template.json'
            fileExists 'run_scanner.sh'
            sh 'chmod +x run_scanner.sh'
            sh './run_scanner.sh -b http://engine-oss.secure-code-box-jannik-ansible.svc:8080 http://elasticsearch.secure-code-box-jannik-ansible.svc:9200 -i 500 -w 2 sslyze https://iteratec.de:443'
            sh 'cat job__sslyze_result.readable'
            archiveArtifacts 'job__sslyze_result.json,job__sslyze_result.readable'
          }
        }
      }
    }
  }
}