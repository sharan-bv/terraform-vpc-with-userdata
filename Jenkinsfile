// Infrastructure Pipelines
pipeline {
    agent any

    stages {
        stage ("Initialize the Terraform"){
            steps {
                sh "terraform init"
            }
        }
        stage ("Validate the tf code"){
            steps {
                sh "terraform validate"
            }
        }
        stage ("Verify the resouces that will be created"){
            steps {
                sh "terraform plan > plan.txt"
                sh "cat plan.txt"
            }
        }
        stage ("Create the complete Infra"){
            input message: 'Approve Terraform Apply?', submitter: 'sharan'
            steps {
                sh "terraform apply -auto-approve > output.txt"
            }
        }
        stage ("Show the outputs"){
            steps {
                sh "cat output.txt"
            }
        }
    }
}