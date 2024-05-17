
pipeline {
    agent any; 
    environment {
       MY_CRED = credentials('azurelogin')
       ARM_TENANT_ID       = "c48e4112-9a8f-418b-a115-e5929b0c550a"
    } 
    stages {
        stage('Git checkout'){
            steps {
                git 'https://github.com/S-I-N-D-H-U-J-A/JenkinTerraformAzure'
            }
        }
        stage('azurelogin') {
            steps {
                bat 'az login --service-principal -u $MY_CRED_CLIENT_ID -p $MY_CRED_CLIENT_SECRET -t $MY_CRED_TENANT_ID'
            }
        }
        stage('Terraform Init'){
            steps{
                    bat """                    
                    echo "Initialising Terraform"
                    terraform init
                    """
            }
        }
        stage('Terraform Validate'){
            steps {
                    bat """                    
                    echo "validating Terraform Code"
                    terraform validate
                    """
            }
        }
        stage('Terraform Plan'){
            steps {
                    withCredentials([azureServicePrincipal(
                    credentialsId: 'azurelogin',
                    subscriptionIdVariable: 'ARM_SUBSCRIPTION_ID',
                    clientIdVariable: 'ARM_CLIENT_ID',
                    clientSecretVariable: 'ARM_CLIENT_SECRET',
                    tenantIdVariable: 'ARM_TENANT_ID'
                )]) {
                        bat """                    
                        echo "Plan Terraform"
                        terraform plan
                        """
                           }
                 }
        }
        stage('Terraform apply') {
            steps {
                withCredentials([azureServicePrincipal(
                credentialsId: 'azurelogin',
                subscriptionIdVariable: 'ARM_SUBSCRIPTION_ID',
                clientIdVariable: 'ARM_CLIENT_ID',
                clientSecretVariable: 'ARM_CLIENT_SECRET',
                tenantIdVariable: 'ARM_TENANT_ID')]){
                    bat """                    
                        echo "Apply Terraform"
                        terraform apply -lock=false -auto-approve
                        """
                }
            }
        }
    }
post {
    failure {
                echo "Jenkins Build Failed"
            }
    
    success {
                echo "Jenkins Build Success"
            }
    always {
        cleanWs()
           }
    }
}
