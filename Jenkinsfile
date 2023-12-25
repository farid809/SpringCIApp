#!/usr/bin/env groovy
def DOCKERHUB_CREDENTIALS // Define DOCKERHUB_CREDENTIALS at the top level

node {
    stage('checkout') {
        checkout scm
    }

    stage('check java') {
        sh "java -version"
    }

    stage('clean') {
        sh "chmod +x gradlew"
        sh "./gradlew clean --no-daemon"
    }
    stage('nohttp') {
        sh "./gradlew checkstyleNohttp --no-daemon"
    }

    stage('npm install') {
        sh "./gradlew npm_install -PnodeInstall --no-daemon"
    }

    stage('packaging') {
        sh "./gradlew bootJar -x test -Pprod -PnodeInstall --no-daemon"
        archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
    }

   environment {
       DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
   




 

   
    stage('publish docker') {
        // A pre-requisite to this step is to setup authentication to the docker registry
        // https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin#authentication-methods
       docker.withRegistry('https://hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        // You can now perform Docker-related actions here
                        // For example, you can build and push Docker images
                    
       sh "./gradlew bootJar jib -Pprod -PjibArchitecture=amd64 -Djib.to.image=farid809/springci  --no-daemon"
    }
}
}
