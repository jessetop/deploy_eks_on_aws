pipeline {
    agent any

    environment {
        PLATFORM = 'linux_amd64'        
    }

    stages {        
        stage('Install kubectl') {
            steps {            
                sh 'echo Installing eksctl'
                sh 'curl -O "https://s3.us-west-2.amazonaws.com/amazon-eks/1.33.0/2025-05-01/bin/linux/amd64/kubectl"'
                sh 'chmod +x ./kubectl'                
                sh 'sudo mv ./kubectl /usr/local/bin/kubectl'     
                //sh 'mkdir -p ~/.local/bin'
                //sh 'mv ./kubectl ~/.local/bin/kubectl'     
                sh 'echo Getting kubectl version'
                sh './kubectl version'                       
            }
        }
        stage('Install eksctl') {
            steps {
                script {
                    sh 'echo Installing eksctl'                       
                    sh 'curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_${PLATFORM}.tar.gz"'                          
                    sh 'tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz'
                    sh 'sudo mv /tmp/eksctl /usr/local/bin'
                    sh 'echo Getting eksctl version'
                    sh 'eksctl version'
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
        //stage('Cleaning Workspace') {
        //    steps {
         //       cleanWs()
          //  }
        //}
    }
    
}