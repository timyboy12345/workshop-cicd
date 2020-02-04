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
                //dir("ci/code") {
                    //sh 'docker-compose -f docker-compose-e2e.yml build';
                    //sh 'docker-compose -f docker-compose-e2e.yml up -d frontend backend';
                    //sh 'docker-compose -f docker-compose-e2e.yml down --rmi=all -v';
                //}
                echo "More tests"
            }
        }
        stage('Deploy') {
            agent {
                docker { image 'node:alpine' }
            }
            steps {              
                sh 'docker-compose -f docker-compose.yml build';
                sh 'docker-compose -f docker-compose.yml up -d';
            }
        }
    }
}
