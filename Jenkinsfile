pipeline {
    agent {label "agent"}
    stages {
        stage('Git') {
            steps {
                echo 'Downoading..'
                git 'https://github.com/VIJAYSPARE/myappcicd.git'
                echo "Code Downloaded Succesfully!"
            }
        }
        stage("Setup Ansible"){
            steps{
                echo "Testing was already done succesfully via Github Workflows"
                sh "yum install ansible -y"
                echo "Ansible Installed"
        }
        }
        stage("Setup Terraform"){
            steps{
                script {
                    if (!fileExists('terraform_1.6.0_linux_amd64.zip')) {
                        sh 'wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip'
                        sh "unzip terraform_1.6.0_linux_amd64.zip"
                        sh "mv terraform /usr/local/bin/"
                    } else {
                        echo 'Terraform zip file already exists. Skipping download.'
                    }
                }
            sh "terraform --version"
            }
        }
        stage("Create Infrastructure for PROD"){
            steps{
            sh "terraform init"
            sh "terraform apply --auto-approve"
            sh "sleep 30" //giving some time for infrastructure to be up and running..
            echo "Infrastructure is up and running.."
            }
        }
        stage("Configure k8s cluster on the created infrastructure "){
            steps{
                sh "chmod 400 vijaykey"
                sh "ansible-playbook k8s_cluster.yml"
                echo "K8s minikube cluster configured succesfully!"
            }
        }
       
        stage("Deploy the Webserver"){
            steps{
                sh "ansible-playbook deployDeployment.yml"
                echo "Create a Socat to connect to our webserver from the Internet(from the outside of ec2 Instance)"
               
            }
        }
    }
}

