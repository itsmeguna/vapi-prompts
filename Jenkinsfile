pipeline {
    agent any

    environment {
        VAPI_URL = 'https://api.vapi.ai/api/assistant'  
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/itsmeguna/vapi-prompts.git'
            }
        }

        stage('Deploy Prompts') {
            steps {
                withCredentials([string(credentialsId: 'VAPI_API_KEY', variable: 'VAPI_KEY')]) {
                    script {
                        def prompts = readFile('ai-prompt.json').trim()
                        sh """
                            curl -X POST \\
                                 -H "Content-Type: application/json" \\
                                 -H "Authorization: Bearer $VAPI_KEY" \\
                                 -d '${prompts}' \\
                                 $VAPI_URL
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
