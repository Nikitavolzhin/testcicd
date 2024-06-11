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
        stage('Build Docker image') {
            steps {
                sh 'docker-compose build'
            }
        }
        stage('Run Docker containers') {
            steps {
                sh 'docker-compose up -d'
            }
        }
        stage('DockerHub') {
            steps {
                sh 'docker push volzhinnikita/ci_cd_test:latest'
            }
        }

    }
}