pipeline {
    agent any

    environment {
        VAPI_URL = 'https://dashboard.vapi.ai/assistants/051fd725-6f8c-47a7-8e79-0d812c1b0536'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/itsmeguna/vapi-prompts.git'
            }
        }

        stage('Deploy Prompt to Assistant') {
            steps {
                withCredentials([string(credentialsId: 'VAPI_API_KEY', variable: 'VAPI_KEY')]) {
                    script {
                        // Print which assistant is being updated
                        echo "Updating Assistant at: ${env.VAPI_URL}"

                        // Use curl to send PATCH request with JSON file
                        sh '''
                            curl -X PATCH \
                                 -H "Content-Type: application/json" \
                                 -H "Authorization: Bearer $VAPI_KEY" \
                                 -d @vapi-prompts/ai-prompt.json \
                                 $VAPI_URL
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo ' Prompt deployed to Vapi successfully!'
        }
        failure {
            echo ' Deployment failed. Check the logs for details.'
        }
    }
}
