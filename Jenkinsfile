pipeline {
    agent any

    environment {
        PLATFORM = 'linux_amd64'        
        BIN_PATH = '/var/lib/jenkins/.local/bin'
    }

    stages {        
        stage('Install kubectl') {
            steps {            
                sh 'echo --------------------------------------------'
                sh 'echo Installing eksctl'
                sh 'echo --------------------------------------------'
                sh 'curl -O "https://s3.us-west-2.amazonaws.com/amazon-eks/1.33.0/2025-05-01/bin/linux/amd64/kubectl"'
                sh 'chmod +x ./kubectl'                                   
                sh 'mkdir -p ~/.local/bin'                
                sh 'mv ./kubectl ~/.local/bin/kubectl'                     
                sh 'echo --------------------------------------------'
                sh 'echo Getting kubectl version'
                sh 'echo --------------------------------------------'
                sh '${BIN_PATH}/kubectl version --client=true'                       
            }
        }
        stage('Install eksctl') {
            steps {
                script {
                    sh 'echo --------------------------------------------'
                    sh 'echo Installing eksctl'                       
                    sh 'echo --------------------------------------------'
                    sh 'curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_${PLATFORM}.tar.gz"'                          
                    sh 'tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz'
                    sh 'mv /tmp/eksctl ~/.local/bin/eksctl'
                    sh 'echo --------------------------------------------'
                    sh 'echo Getting eksctl version'
                    sh 'echo --------------------------------------------'
                    sh '${BIN_PATH}/eksctl version'
                }                
            }
        }        
        stage('Create EKS Cluster') {
            steps {
                sh 'echo --------------------------------------------'
                sh 'echo ----- Creating EKS Cluster -----------------'
                sh 'echo --------------------------------------------'
                sh '${BIN_PATH}/eksctl create cluster -f cluster_config.yaml --verbose 4'       
                
            }
            post {
            // if EKS Cluster create fails, try and delete it so we don't leave it in an inconsistent state
                failure {
                    sh 'echo --------------------------------------------'
                    sh 'echo ----- Deleting any orphaned resources ------'
                    sh 'echo --------------------------------------------'
                    sh '${BIN_PATH}/eksctl delete cluster -f cluster_config.yaml --verbose 4'       
                }
            }
        }        
        stage('Update kubeconfig') {
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