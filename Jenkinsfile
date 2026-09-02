pipeline {
    agent any

    parameters {
        choice(name: 'ACTION', choices: ['apply', 'destroy'], description: 'Terraform action to run')
    }

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    stages {
        stage('checkout code') {
            steps {
                echo "checking out code from GitHub"
                git branch: 'main', url: 'https://github.com/Akshatha0915/TERRAFORM-PROJECT.git'
            }
        }

        stage('Terraform init') {
            steps {
                echo "Initializing Terraform"
                sh 'terraform init'
            }
        }

        stage('Terraform validate') {
            steps {
                echo "validating Terraform configuration"
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                echo "Running Terraform Plan"
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                echo "Applying Terraform configuration"
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }

    post {
        success {
            echo "Terraform Apply completed successfully"
        }
        failure {
            echo "Terraform action failed"
        }
    }
}
