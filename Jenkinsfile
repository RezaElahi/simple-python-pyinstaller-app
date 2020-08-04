pipeline{
  agent none
  stages{
    stage("Setup"){
      agent{
        docker{
          image "python:2-alpine"
        }
      }
      steps{
        sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
        stash(name: 'compiled-results', includes: 'sources/*.py*') 
        echo "setup stage is finished!"
      }
    }
    stage("Test"){
      agent{
        docker{
          image "qnib/pytest"
        }
      }
      steps{
        sh "py.test --junit-xml test-reports/results.xml sources/test_calc.py"
      }
      post{
        always{
          junit 'test-reports/results.xml' 
        }
      }
    }
  }
}
