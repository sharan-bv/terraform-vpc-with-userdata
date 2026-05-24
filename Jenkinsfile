// Infrastructure Pipelines
pipeline {
    agent any

    stages {
        stage("Run Terraform Workflow") {
            steps {
                // Wrapping all steps globally so credentials persist across init, validate, plan, and apply
                withCredentials([usernamePassword(credentialsId: 'aws_creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    
                    echo "Starting Terraform Initialization..."
                    sh "terraform init"
                    
                    echo "Validating Configuration..."
                    sh "terraform validate"
                    
                    echo "Generating Plan..."
                    sh "terraform plan > plan.txt"
                    sh "cat plan.txt"
                }
            }
        }
        
        stage("Approval Gate") {
            input {
                message "Should we deploy the infrastructure?"
                submitter "sharan"
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws_creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    echo "Applying Changes..."
                    sh "terraform apply -auto-approve > output.txt"
                    sh "cat output.txt"
                }
            }
        }
    }
}