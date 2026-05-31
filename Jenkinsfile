// Infrastructure Pipelines
pipeline {
    agent any

    stages {
        stage("Initialize the Terraform") {
            steps {
                // This explicitly binds the username and password fields to standard env variables
                withCredentials([usernamePassword(credentialsId: 'aws_creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
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
                withCredentials([usernamePassword(credentialsId: 'aws_creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh "terraform plan > plan.txt"
                    sh "cat plan.txt"
                }
            }
        }
        
        stage("Create the complete Infra") {
            input {
                message "Should we deploy the infrastructure?"
                submitter "sharan"
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws_creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
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