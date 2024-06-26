pipeline {
    agent any 
    environment {
       MY_CRED = credentials('e9138bd2-49ab-47d3-92d4-1e536603e57a')
       PATH = "C:\\terraform;${env.PATH}"
    } 
    stages {
        stage('Git checkout') {
            steps {
                git 'https://github.com/S-I-N-D-H-U-J-A/JenkinTerraformAzure'
            }
        }

        stage('Check PATH') {
            steps {
                bat 'echo %PATH%'
            }
        }
        
        stage('Terraform Init') {
            steps {
                echo 'Initialising Terraform'
                bat 'C:\\terraform\\terraform.exe init'
            }
        }

        stage('Terraform Validate') {
            steps {
                echo 'Validating Terraform'
                bat 'C:\\terraform\\terraform.exe validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                withCredentials([azureServicePrincipal(
                    credentialsId: 'azurelogin',
                    subscriptionIdVariable: 'ARM_SUBSCRIPTION_ID',
                    clientIdVariable: 'ARM_CLIENT_ID',
                    clientSecretVariable: 'ARM_CLIENT_SECRET',
                    tenantIdVariable: 'ARM_TENANT_ID'
                )]) {
                    echo "Plan Terraform"
                    bat 'C:\\terraform\\terraform.exe plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                withCredentials([azureServicePrincipal(
                    credentialsId: 'azurelogin',
                    subscriptionIdVariable: 'ARM_SUBSCRIPTION_ID',
                    clientIdVariable: 'ARM_CLIENT_ID',
                    clientSecretVariable: 'ARM_CLIENT_SECRET',
                    tenantIdVariable: 'ARM_TENANT_ID'
                )]) {
                    echo "Apply Terraform"
                    bat 'C:\\terraform\\terraform.exe apply -auto-approve tfplan'
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
