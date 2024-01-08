pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'darkages89/gitops:hellov1.3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Set up Python 3.7') {
            steps {
                script {
                    // You may need to install Python and pip here if not already available
                    sh 'python -m pip install --upgrade pip'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'pip install -r python/requirements.txt'
                }
            }
        }

        stage('Build & Push Image') {
            steps {
                script {
                    dir('python') {
                        withCredentials([usernamePassword(credentialsId: 'DOCKER_CREDENTIALS_ID', passwordVariable: 'DOCKERPW', usernameVariable: 'DOCKERPW')]) {
                            sh "echo \${DOCKERPW} | docker login -u \${DOCKERPW} --password-stdin"
                        }
                        sh "docker image build -t ${DOCKER_IMAGE_NAME} ."
                        sh "docker push ${DOCKER_IMAGE_NAME}"
                    }
                }
            }
        }
    }
}
