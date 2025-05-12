pipeline {
    agent any

    environment {
        CONFIG_FILE = 'config.env'
        PROMPT_FILE = 'ai-prompt.json'
    }

    stages {
        stage('Load Config & Update Prompt') {
            steps {
                script {
                    // Load .env file into environment variables
                    def config = readProperties file: CONFIG_FILE
                    def promptPayload = readFile(PROMPT_FILE).replaceAll("'", "'\\\\''")

                    def apiUrl = "https://api.vapi.ai/assistant/${config.ASSISTANT_ID}"

                    sh """
                        curl --location --request PATCH '${apiUrl}' \\
                        --header 'Authorization: Bearer ${config.VAPI_TOKEN}' \\
                        --header 'Content-Type: application/json' \\
                        --data '${promptPayload}'
                    """
                }
            }
        }
    }
}
