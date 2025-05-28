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
                echo "Installing eksctl"
                sh 'curl -O "https://s3.us-west-2.amazonaws.com/amazon-eks/1.33.0/2025-05-01/bin/linux/amd64/kubectl"'
                sh 'chmod +x ./kubectl'                                   
                sh 'mkdir -p ~/.local/bin'                
                sh 'mv ./kubectl ~/.local/bin/kubectl'                                     
                echo "Getting kubectl version"                
                sh '${BIN_PATH}/kubectl version --client=true'                       
            }
        }
        stage('Install eksctl') {
            steps {
                script {
                    // need to add a check to see if the file exists before installing, or check version against latest version                    
                    echo "Installing eksctl"
                    sh 'curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_${PLATFORM}.tar.gz"'                          
                    sh 'tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz'
                    sh 'mv /tmp/eksctl ~/.local/bin/eksctl'                    
                    echo "Getting eksctl version"
                    sh '${BIN_PATH}/eksctl version'
                }                
            }
        }        
        stage('Create EKS Cluster') {
            steps {                
                echo "Creating EKS Cluster"
                sh '${BIN_PATH}/eksctl create cluster -f cluster_config.yaml'       
                
            }
            post {                
                failure {
                    script {
                        try {
                        // if EKS Cluster create fails, try and delete it so we don't leave it in an inconsistent state
                        // this is hardcoded and needs to be paramterized later
                        sh 'aws cloudformation delete-stack --region us-east-1 --stack-name eksctl-test-cluster-name-cluster'                                                                                                
                        sh 'aws cloudformation wait stack-delete-complete --region us-east-1 --stack-name eksctl-test-cluster-name-cluster'
                        }
                        catch (Exception e) {                            
                                echo "Failed to delete Cloudformation Stack: " + e.getMessage()
                                echo "Forcing deletion of Cloudformation Stack"
                                sh 'aws cloudformation delete-stack --deletion-mode FORCE_DELETE_STACK --region us-east-1 --stack-name eksctl-test-cluster-name-cluster'                                
                                sh 'aws cloudformation wait stack-delete-complete --region us-east-1 --stack-name eksctl-test-cluster-name-cluster'
                        }                    
                        finally {

                            try {
                                // check that the cluster exists first using aws eks describe-cluster
                                // sh 'aws eks describe-cluster --name test-cluster-name --region us-east-1'
                                // sh '${BIN_PATH}/eksctl delete cluster -f cluster_config.yaml'       
                            }
                            catch(Exception e) {
                                echo "Failed to delete EKS Cluster:" + e.getMessage()
                            }
                        }                                                   
                    }                    
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