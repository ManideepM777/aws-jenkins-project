#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/ManideepM777/jenkins-shared-library.git',
     credentialsId: 'github'
    ]
)

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        IMAGE_NAME = 'manideepm777/java-app:1.0'
    }
    stages {
        stage('build app') {
            steps {
               script {
                  echo 'building application jar...'
                  buildJar()
               }
            }
        }
        stage('build image') {
            steps {
                script {
                   echo 'building docker image...'
                   buildImage(env.IMAGE_NAME)
                   dockerLogin()
                   dockerPush(env.IMAGE_NAME)
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                   echo "deploying the application"
                   def dockerCmd = "docker run -p 8080:8080 -d ${IMAGE_NAME}"
                   sshagent(['aws-sshkey']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@43.204.101.30 ${dockerCmd}"
                   }
                }
            }
        }
    }
}
