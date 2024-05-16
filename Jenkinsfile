pipeline {
    agent any; 
    environment {
       MY_CRED = credentials('azurelogin')
       ARM_CLIENT_ID     = "1e79ea61-0414-4937-9ab8-92f97632f1ca"
       ARM_CLIENT_SECRET = "PKf8Q~ZBsiKVSE1v91gCLL_LSwCCF6h3.uNsFaYg"
       ARM_SUBSCRIPTION_ID = "2c232cc6-1a70-4669-bd20-3ade791c35ce"
       ARM_TENANT_ID = "c48e4112-9a8f-418b-a115-e5929b0c550a"
    } 
    stages {
        stage('Git checkout'){
            steps {
                git 'https://github.com/S-I-N-D-H-U-J-A/JenkinTerraformAzure'
            }
        }
        stage('azurelogin') {
            steps {
                sh 'az login --service-principal -u $ARM_CLIENT_ID -p $ARM_CLIENT_SECRET -t $ARM_TENANT_ID'
            }
        }
        stage('Terraform Init'){
            steps{
                    sh """                    
                    echo "Initialising Terraform"
                    terraform init
                    """
            }
        }
        stage('Terraform Validate'){
            steps {
                    sh """                    
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
                        sh """                    
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
                    sh """                    
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
