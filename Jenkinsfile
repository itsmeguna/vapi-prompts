pipeline {
    agent any

    environment {
        VAPI_URL = 'https://api.vapi.ai/assistants/051fd725-6f8c-47a7-8e79-0d812c1b0536'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/itsmeguna/vapi-prompts.git'
            }
        }

        stage('Update System Prompt') {
            steps {
                withCredentials([string(credentialsId: 'VAPI_API_KEY', variable: 'VAPI_KEY')]) {
                    script {
                        def newPrompt  = readFile('ai-prompt.json').trim()
                        def payload = """
                        {
                          "model": {
                            "messages": [
                              {
                                "role": "system",
                                "content": ${groovy.json.JsonOutput.toJson(newPrompt)}
                              }
                            ]
                          }
                        }
                        """
                        writeFile file: 'payload.json', text: payload

                        sh '''
                            curl -X PATCH \
                                 -H "Authorization: Bearer $VAPI_KEY" \
                                 -H "Content-Type: application/json" \
                                 --data @payload.json \
                                 $VAPI_URL
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo ' Prompt update successful!'
        }
        failure {
            echo ' Prompt update failed.'
        }
    }
}
