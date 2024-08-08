#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    echo "Testing the application"
                }
            }
        }
        stage("build") {
            steps {
                script {
                    echo "building the application"
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    echo "deploying the application"
                    def dockerCmd = 'docker run -p 80:80 -d manideepm777/node-example.1.0:latest'
                    sshagent(['aws-sshkey']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@15.206.203.138 ${dockerCmd}"
                    }
                }
            }
        }
    }   
}
