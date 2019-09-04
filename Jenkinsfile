pipeline {
    agent { label 'kube'}
    stages {
        stage('My Stage') {
            steps {
                script {
                    def SERVICES = sh (script: "kubectl get deploy --all-namespaces | awk '{print $2}' | sed -n '1!p'", returnStdout:true).trim()
                    inputResult = input(
                        message: "Select a service",
                        parameters: [choice(name: 'Service to deploy', choices: "${SERVICES}", description: 'What service you wont deploy?')]
                    )
                }
            }
        }
        stage('My other Stage'){
            steps{
                echo "The selected service is: ${inputResult}"
            }
        }
        stage('Stage-Two'){
            steps {
               sh "echo ${inputResult}"
            }
        }
    }
}
