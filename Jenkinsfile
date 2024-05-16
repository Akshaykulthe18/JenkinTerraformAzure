pipeline {
    agent any; 
    environment {
       MY_CRED = credentials('azurelogin')
    } 
    stages {
        stage('Git checkout') {
            steps {
                git 'https://github.com/S-I-N-D-H-U-J-A/JenkinTerraformAzure'
            }
        }
        stage('azurelogin') {
            steps {
                sh 'az login --service-principal -u $MY_CRED_CLIENT_ID -p $MY_CRED_CLIENT_SECRET -t $MY_CRED_TENANT_ID'
            }
        }
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }
        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }
        stage('Terraform apply') {
            steps {
                sh 'terraform apply -lock=false -auto-approve'
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
