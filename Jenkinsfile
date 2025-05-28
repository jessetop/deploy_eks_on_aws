pipeline {
    agent any

    environment {
        PLATFORM = 'linux_amd64'        
        BIN_PATH = '/var/lib/jenkins/.local/bin'
    }

    stages {        
        stage('Install kubectl') {
            steps {            
                // need to add a check to see if the file exists before installing, or check version against latest version
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
                    // need to add a check to see if the file exists before installing, or check version against latest version
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
                sh '${BIN_PATH}/eksctl create cluster -f cluster_config.yaml'       
                
            }
            post {
            // if EKS Cluster create fails, try and delete it so we don't leave it in an inconsistent state
                failure {
                    // this is hardcoded and needs to be paramterized later
                    sh 'aws cloudformation delete-stack --deleteion-mode FORCE_DELETE_STACK --region-us-east-1 --stack-name eksctl-test-cluster-name-cluster'                    
                    sh '${BIN_PATH}/eksctl delete cluster -f cluster_config.yaml'       
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