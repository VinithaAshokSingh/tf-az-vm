pipeline {
    agent any

       environment {
        HOME = "/var/lib/jenkins"
        TF_PLUGIN_CACHE_DIR = "/var/lib/jenkins/.terraform.d/plugin-cache"
        TF_PLUGIN_TIMEOUT   = "15m"

        // Set to ERROR for CI (TRACE only for debugging)
        TF_LOG      = "ERROR"
        TF_LOG_PATH = "terraform-debug.log"

        ARM_CLIENT_ID       = credentials('ARM_CLIENT_ID')
        ARM_CLIENT_SECRET   = credentials('ARM_CLIENT_SECRET')
        ARM_TENANT_ID       = credentials('ARM_TENANT_ID')
        ARM_SUBSCRIPTION_ID = credentials('ARM_SUBSCRIPTION_ID')
        
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                git(
                    branch: 'main',
                    credentialsId: 'github-pat',
                    url: 'https://github.com/VinithaAshokSingh/tf-az-vm.git'
                )
            }
        }

        stage('Terraform Init') {
            steps {
                sh '''
                    rm -rf .terraform
                    terraform init -upgrade
                '''
            }
        }

        stage('Terraform Plan') {
            steps {
                sh '''
                    terraform plan 
                '''
            }
        }

        stage('Terraform Apply') {
            steps {
                sh '''
                    terraform apply -auto-approve tfplan
                '''     
            }
        }
    }
}
