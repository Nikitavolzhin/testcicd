pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                sh 'python3 -m py_compile add2vals.py calc.py'
                stash(name: 'compiled-results', includes: '*.py*')
            }
        }

    }
}