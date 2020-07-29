pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Setup Terraform Variables') {
            when { branch 'PR-*' }
            steps {
                sh 'cp ../terraform.tfvars .'
            }
        }
        stage('Terraform Init') {
            when { branch 'PR-*' }
            steps {
                sh '/usr/local/bin/terraform init -backend-config=backend.tf'
            }
        }
        stage('Terraform Plan') {
            when { branch 'PR-*' }
            steps {
                sh '/usr/local/bin/terraform plan -out=myplan'
            }
        }
        stage('Terraform Approval') {
            when { branch 'PR-*' }
            steps {
                script {
                    def userInput = input(id: 'confirm', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'confirm'] ])
                }
            }
        }
        stage('Terraform Apply') {
            when { branch 'PR-*' }
            steps {
                sh '/usr/local/bin/terraform apply -input=false myplan'
            }
        }
        stage('Show Build Results') {
            when { branch 'PR-*' }
            steps {
                sh '/usr/local/bin/terraform show'
            }
        }
    }
}