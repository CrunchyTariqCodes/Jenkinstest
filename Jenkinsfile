pipeline {
    agent any

    parameters {
        string(name: 'AZURE_ENV', description: 'Azure environment to log into', defaultValue: '')
        string(name: 'IMAGE_TAG', description: 'Image tag to check and delete', defaultValue: '')
        string(name: 'ACR_NAME', description: 'Azure Container Registry name', defaultValue: '')
        string(name: 'DEVELOPER_EMAIL', description: 'Developer\'s email for notification', defaultValue: '')
        string(name: 'APPROVER_EMAIL', description: 'Approver\'s email for notification', defaultValue: '')
    }

    stages {
        stage('Git Credential Setup') {
            steps {
                // Define the Git credentials using the Jenkins credentials plugin
                withCredentials([usernamePassword(credentialsId: 'your_git_credentials_id', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    // Configure Git with the provided credentials
                    git credentialsId: 'your_git_credentials_id', url: 'https://github.com/yourusername/your-repo.git'
                }
            }
        }

        stage('Approval') {
            steps {
                script {
                    // Request approval from two distinct approvers
                    def inputMessage = 'Please approve the build.'
                    def approver1 = input(message: inputMessage, submitter: 'approver1')
                    def approver2 = input(message: inputMessage, submitter: 'approver2')

                    // Check if both approvers have approved the build
                    if (approver1 && approver2) {
                        echo 'Build approved by both approver1 and approver2. Proceeding...'
                    } else {
                        error 'Build approval requires approval from both approver1 and approver2.'
            }
        }
    }
}

        stage('Azure Login') {
            steps {
                script {
                    // This stage logs into the specified Azure environment
                    echo "Logging into Azure environment: ${params.AZURE_ENV}"
                    // Implement your Azure login logic here
                    // e.g., sh "az login ..."
                }
            }
        }

        stage('Check and Delete Image') {
            steps {
                script {
                    def imageTag = params.IMAGE_TAG
                    def acrName = params.ACR_NAME

                    echo "Checking image:tag ${imageTag} in ACR ${acrName}"

                    // Check if the image with the specified tag exists in ACR
                    def imageExists = sh(script: "az acr repository show-tags --name ${acrName} --repository ${imageTag} --output tsv", returnStdout: true).trim()
                    if (imageExists != '') {
                        echo "Image ${imageTag} found in ACR. Deleting..."
                        // Delete the image
                        sh "az acr repository delete --name ${acrName} --repository ${imageTag} --yes"
                        echo "Image ${imageTag} deleted from ACR."
                    } else {
                        echo "Image ${imageTag} not found in ACR."
                    }
                }
            }
        }

        stage('Notification') {
            steps {
                script {
                    // This stage sends notification emails to the developer and approver
                    mail to: params.DEVELOPER_EMAIL,
                         subject: "Image Deletion Status",
                         body: "Image deletion status: Add appropriate response here."

                    mail to: params.APPROVER_EMAIL,
                         subject: "Image Deletion Status",
                         body: "Image deletion status: Add appropriate response here."
                }
            }
        }
    }

    post {
        always {
            // Clean up or perform actions that should always happen
        }
        success {
            // Actions to be taken on successful build
            echo 'Pipeline succeeded!'
        }
        failure {
            // Actions to be taken on failed build
            echo 'Pipeline failed!'
        }
    }
}
