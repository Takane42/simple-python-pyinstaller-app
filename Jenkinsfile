def remote = [:]
remote.name = "serverProd"
remote.host = "13.212.229.162"
remote.allowAnyHosts = true

node {
  checkout scm

  stage('Build') {
  withDockerContainer('python:2-alpine') {
    sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    } 
  }

  stage('Test') {
  try {
  withDockerContainer('qnib/pytest') {
    sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
      }
  finally {
    junit 'test-reports/results.xml'
    }
  }

  stage('Manual Approval'){
  input "Lanjutkan ke tahap Deploy?"
  }

  withCredentials([sshUserPrivateKey(credentialsId: 'sshAWS', keyFileVariable: '', passphraseVariable: 'password', usernameVariable: 'userName')]) {
  remote.user = userName
  remote.password = password
  stage('Deploy'){
        sshPut remote: remote, from: '*', into: '.'
        sshCommand remote: remote, command: 'ls'
    }
  }

}

