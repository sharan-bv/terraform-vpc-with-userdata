// Infrastructure Pipelines

pipeline {
    agent any

    stages {

        stage('git checkout') {
            steps {
                echo "Check out the code from the defined GITHub repo"
            }
        }

        stage("Terraform Execution") {
            steps {

                // 🔐 ONE TIME AWS CREDENTIALS FOR ALL TERRAFORM STEPS
                withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding',
                     credentialsId: 'aws-creds']
                ]) {

                    sh "terraform init"

                    sh "terraform validate"

                    sh "terraform plan > plan.txt"
                    sh "cat plan.txt"

                    input message: 'Approve Terraform Apply?', submitter: 'sharan'

                    sh "terraform apply -auto-approve > output.txt"

                    sh "cat output.txt"
                }
            }
        }

        stage("Show outputs") {
            steps {
                sh "cat output.txt"
            }
        }
    }
}