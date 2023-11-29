pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'your-region' // Replace with your AWS region
        STACK_NAME = 'your-stack-name'     // Replace with your CloudFormation stack name
        PARAMETERS_FILE = 'path/to/your/updated-parameters.json' // Replace with the path to your updated CloudFormation parameters file
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Update Stack Parameters') {
            steps {
                script {
                    // Update CloudFormation stack parameters
                    sh "aws cloudformation update-stack --stack-name ${STACK_NAME} --use-previous-template --parameters file://${PARAMETERS_FILE}"

                    // Wait for the stack update to complete
                    sh "aws cloudformation wait stack-update-complete --stack-name ${STACK_NAME}"
                }
            }
        }
    }

    post {
        always {
            // Cleanup or perform any other post-build tasks
        }
    }
}
