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


  stage('Deploy'){
    'sh scp * jenkins@13.212.229.162:~/python/'
    'sh ssh jenkins@13.212.229.168 "docker run -it -v "$(pwd):/src/" -w /src cdrx/pyinstaller-linux:python2 "python -m py_compile sources/add2vals.py sources/calc.py && pyinstaller --onefile sources/add2vals.py""'
    }
  }


