pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_NAME = 'your-jhipster-app'
        DOCKER_IMAGE_TAG = "nightly"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('check java') {
            steps {
                sh 'java -version'
            }
        }

       stage('clean') {
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew clean --no-daemon'
            }
        }

        stage('nohttp') {
            steps {
                sh './gradlew checkstyleNohttp --no-daemon'
            }
        }

        stage('npm install') {
          steps {
             sh "./gradlew npm_install -PnodeInstall --no-daemon"
        }
        }

        stage('packaging') {
            steps {
               sh "./gradlew bootJar -x test -Pprod -PnodeInstall --no-daemon"
               archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}", '.')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.example.com', 'docker-hub-credentials') {
                        dockerImagePush("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
    }
}

