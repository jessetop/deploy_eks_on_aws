pipeline {
    agent any

    stages {
        stage('Install eksctl') {
            steps {
                echo 'Installing eksctl'
                curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.33.0/2025-05-01/bin/linux/amd64/kubectl
                chmod +x ./kubectl
                mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
                echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
            }
        }
        stage('Install kubectl') {
            steps {
                echo 'Installing kubectl'                
                sudo yum install kubectl
            }
        }        
        stage('Create EKS Cluster') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}