pipeline {

    agent any

    stages {
        stage("zero - does nothing but checks that we are on the jenkis server"){
            steps {
                sh "echo ${USER}"
                sh "df -h"
                sh "curl ifconfig.co"
                sh "echo the above is the jenkins IP"
            }
        

        stage("connect to deploy server"){

            environment { 
                SSH_CRED = credentials('jenkinstest-pem')
            }

            steps {
                
                script {
                    sh """
                    #!/bin/bash
										
					echo "connecting to remote or deploy server"	
                    ssh -i $SSH_CRED -t -o StrictHostKeyChecking=no ubuntu@ec2-35-182-86-90.ca-central-1.compute.amazonaws.com << EOF
                    curl ifconfig.co/ip
                    df -h
                    echo "running apt update"
                    sudo apt update

                    echo "installing nodejs"
                    sudo apt install nodejs -y

                    echo "installing NPM"
                    sudo apt install npm -y

                    echo "installing yarn"
                    sudo npm install -g yarn -y

                    echo "installing pm2"
                    sudo npm install pm2 -g

                    echo "installing project"
                    sudo mkdir app
                    cd app
                    sudo git clone https://github.com/tolaoguntunde/nest_js_app.git .
                    sudo npm install -y

                    echo "Building project"
                    sudo yarn build

                    echo "changing directory"
                    cd dist

                    echo "starting project in directory"
                    sudo pm2 start main.js

                    echo "list running apps"
                    sudo pm2 list
                    
                    echo "save running apps"
                    sudo pm2 save
                    
                    echo "exiting terminal"
                    exit
                    << EOF
                    """
                }
	    }
	}
	}
    }
	
