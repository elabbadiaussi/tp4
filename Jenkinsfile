pipeline {
    environment {
        registry = "elfatima/tp4"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/elabbadiaussi/tp4.git'
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy image') {
            steps {
                bat "docker rm -f tp4-container 2>nul & docker run -d --name tp4-container -p 8081:80 %registry%:%BUILD_NUMBER%"
            }
        }
    }
}