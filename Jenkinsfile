pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                sh 'python3 -m py_compile add2vals.py calc.py'
                stash(name: 'compiled-results', includes: '*.py*')
            }
        }
        stage('Test') {
                steps {
                    sh 'py.test --junit-xml test-reports/results.xml test_calc.py'
                }
                post {
                    always {
                        junit 'test-reports/results.xml'
                    }
                }
            }
        stage('Building Docker image') {
            steps {
                script {
                    docker.withServer('unix:///var/run/docker.sock') {
                        sh 'docker compose up --build'
                    }
                }
            }
        }
    }
}