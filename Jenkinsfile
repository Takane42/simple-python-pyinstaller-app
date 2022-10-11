def remote = [:]
remote.name = "serverProd"
remote.host = "13.229.120.216"
remote.allowAnyHosts = true
withCredentials([sshUserPrivateKey(credentialsId: 'sshAWS', keyFileVariable: '', passphraseVariable: 'password', usernameVariable: 'userName')]) {
  remote.user = userName
  remote.password = password
}

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

  stage('Deploy'){
  withDockerContainer('cdrx/pyinstaller-linux:python2'){
    sh 'pyinstaller --onefile sources/add2vals.py'
  }
  }
}

