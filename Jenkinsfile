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
                    // This stage is for manual approval before proceeding
                    input message: 'Please approve the build.', submitter: 'approver'
                }
            }
        }

        // Rest of the stages remain unchanged

        // Add the Check and Delete Image stage here

        // Add the Notification stage here
    }

    post {
        // Post-build actions remain unchanged
    }
}
