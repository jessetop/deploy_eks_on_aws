pipeline {
    agent any

    stages {
        stage('Cleaning Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Install eksctl') {
            steps {            
                sh 'echo Installing eksctl'
                sh 'curl -O "https://s3.us-west-2.amazonaws.com/amazon-eks/1.33.0/2025-05-01/bin/linux/amd64/kubectl"'
                sh 'chmod +x ./kubectl'
                sh 'mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH'
                sh "echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc"        
                sh 'echo Getting eksctl version'
                sh 'eksctl version'                       
            }
        }
        stage('Install kubectl') {
            steps {
                script {
                    sh 'echo Installing kubectl'                
                    sh 'sudo yum install kubectl'
                    sh 'echo Getting kubectl version'
                    sh 'kubectl version'
                }                
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