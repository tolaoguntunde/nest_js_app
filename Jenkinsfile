pipeline {

    agent any

    stages {
        stage("environment preparation"){
            steps {
                sh "pwd"
                sh "ls"
                sh "echo ${USER}"
                sh "df -h"
                sh "curl ifconfig.co"
                sh "echo testing"
            }
        }

        stage("connect to deploy server"){

            environment { 
                SSH_CRED = credentials('jenkinstest')
            }

            steps {
                //=============== THIRD APPROACH
                script {
                    sh """
                    #!/bin/bash
                    ssh -i $SSH_CRED -t -o StrictHostKeyChecking=no ubuntu@ec2-3-99-241-196.ca-central-1.compute.amazonaws.com << EOF
                    curl ifconfig.co/ip
                    df -h
                    exit
                    << EOF
                    """
                }
                
            }
        }
    }
}
