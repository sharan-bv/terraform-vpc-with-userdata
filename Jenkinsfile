# Infrastructure Pipelines
pipeline {
    agent any

    environment {
        // This automatically binds your Jenkins AWS credentials to the env variables Terraform looks for
        AWS_CREDS = credentials('aws_creds')
    }

    stages {
        stage("Initialize the Terraform") {
            steps {
                // We wrap the terraform commands using the credentials environment variables
                withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_CREDS_USR}", "AWS_SECRET_ACCESS_KEY=${env.AWS_CREDS_PSW}"]) {
                    sh "terraform init"
                }
            }
        }
        
        stage("Validate the tf code") {
            steps {
                sh "terraform validate"
            }
        }
        
        stage("Verify the resources that will be created") {
            steps {
                withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_CREDS_USR}", "AWS_SECRET_ACCESS_KEY=${env.AWS_CREDS_PSW}"]) {
                    sh "terraform plan > plan.txt"
                    sh "cat plan.txt" // Fixed: Added missing quotes around the command
                }
            }
        }
        
        stage("Create the complete Infra") {
            // Fixed: Input steps in Declarative pipelines need a 'message' parameter
            input {
                message "Should we deploy the infrastructure?"
                submitter "sharan"
            }
            steps {
                withEnv(["AWS_ACCESS_KEY_ID=${env.AWS_CREDS_USR}", "AWS_SECRET_ACCESS_KEY=${env.AWS_CREDS_PSW}"]) {
                    sh "terraform apply -auto-approve > output.txt"
                }
            }
        }
        
        stage("Show the outputs") {
            steps {
                sh "cat output.txt"
            }
        }
    }
}