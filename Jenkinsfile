pipeline {
    agent any

       environment {
       AZURE_CLIENT_ID       = credentials('ARM_CLIENT_ID')
        AZURE_CLIENT_SECRET   = credentials('ARM_CLIENT_SECRET')
        AZURE_TENANT_ID       = credentials('ARM_TENANT_ID')
        AZURE_SUBSCRIPTION_ID = credentials('ARM_SUBSCRIPTION_ID')

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

        stage('Prepare Terraform Cache') {
    steps {
        sh '''
            mkdir -p /var/lib/jenkins/.terraform.d/plugin-cache
            chmod -R 777 /var/lib/jenkins/.terraform.d
        '''
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
            terraform plan -input=false
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
