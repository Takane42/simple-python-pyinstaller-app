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
  withDockerContainer('python:2-alpine'){
    sh 'pip install pyinstaller==3.6'
    sh 'pyinstaller --onefile sources/add2vals.py'
    }
  sh 'scp dist/add2vals jenkins@13.212.229.162'
  sh 'ssh jenkins@13.212.229.162 "./add2vals 2 5"'
  }

//  stage('Deploy'){
//    sh 'scp -r * jenkins@13.212.229.162:./python'
//    sh 'ssh jenkins@13.212.229.162 "ls ./python"'
//    }
}

