
pipeline {
    agent any; 
    environment {
       MY_CRED = credentials('azurelogin')
       
    } 
    stages {
        stage('Git checkout'){
            steps {
                git 'https://github.com/S-I-N-D-H-U-J-A/JenkinTerraformAzure'
            }
        }
        
        stage('Terraform Init'){
            steps{
                    bat """                    
                    echo "Initialising Terraform"
                    bat 'terraform init'
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
