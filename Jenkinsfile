pipeline {
    agent {
        label 'master'
    }
    stages {
        stage('Prepare') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                dir("code/backend") {
                    sh 'npm install';
                }
            }
        }
        stage('Build') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                dir("code/backend") {
                    sh 'npm run build';
                }
            }
        }
        stage('Static Analysis') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                dir("code/backend") {
                    sh 'npm run lint';
                }
            }
        }
        stage('Unit Test') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {
                dir("code/backend") {
                    sh 'npm run test'
                }
            }
        }
        stage('e2e Test') {
            steps {             
                sh 'docker-compose -f docker-compose.yml build';
                sh 'docker-compose -f docker-compose-e2e.yml build';
                sh 'docker-compose -f docker-compose.yml up -d';
                sh 'docker-compose -f docker-compose-e2e.yml up -d frontend backend';
                sh 'docker-compose -f docker-compose-e2e.yml down --rmi=all -v';
            }
            post {
                always {
                     script {
                       sh 'docker-compose -f docker-compose-e2e.yml up e2e'
                       status_code = sh ( script: "docker inspect code_e2e_1 --format='{{.State.ExitCode}}'", returnStdout: true).trim();
                       if (status_code == '1'){
                          error('e2e test failed.')
                       }
                    }
                }
            }
        }
        stage('Deploy') {
            steps {                
                echo 'Deploy'
            }
        }
    }
}
