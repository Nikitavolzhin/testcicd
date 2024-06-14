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

        stage('Push Docker image to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub_password', variable: 'docker_hub_password')]) {
                    sh 'docker login -u volzhinnikita -p ${docker_hub_password}'
                }
                sh 'docker push volzhinnikita/test_cicd'
            }
        }
        stage('Deploying Docker container to K8s') {
            steps {
            sh 'ls'
                script{
                    kubernetesDeploy(configs: "deployment.yaml",
                                         "service.yaml")
                    }
                //sh 'kubectl apply -f deployment.yaml --validate=false'
                //sh 'kubectl apply -f service.yaml --validate=false'
            }
        }

    }
}