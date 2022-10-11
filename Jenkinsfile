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
  sh 'docker run -v "$(pwd):/src/" -w /src cdrx/pyinstaller-linux:python2 "python -m py_compile sources/add2vals.py sources/calc.py && pyinstaller --onefile sources/add2vals.py"'
  sh 'ls'
  sh 'ssh jenkins@13.212.229.162 "rm add2vals || echo "File Not Exists""'
  sh 'scp dist/add2vals jenkins@13.212.229.162:~/add2vals'
  sh 'ssh jenkins@13.212.229.162 "./add2vals 2 5"'
  sleep 60
  }

}

