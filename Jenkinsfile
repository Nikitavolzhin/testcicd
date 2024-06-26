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
        stage('Run AWSCLI') {
            steps {
            withCredentials([string(credentialsId: 'aws_key', variable: 'aws_key')]) {
                sh 'docker-compose run awscli --env AWS_SECRET_ACCESS_KEY=${aws_key} AWS_DEFAULT_REGION=eu-north-1 AWS_ACCESS_KEY_ID=AKIAXYKJQVMXB7I3FBOC'//--rm awscli'
                }
            }
        }

        stage('running python file') {
            steps {
               sh 'docker-compose exec server python3 hellow_world.py'
            }
        }
    }
}
