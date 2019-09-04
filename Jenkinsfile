pipeline {
    agent { label 'kube'}
    stages {
        stage('My Stage') {
            steps {
                script {
                    def SERVICES = sh (script: "kubectl get svc --all-namespaces --sort-by=.metadata.name | awk '{print \$2}' | sed -n '1!p'", returnStdout:true).trim()
                    def DEPLOY = sh (script: "kubectl get deploy --all-namespaces --sort-by=.metadata.name | awk '{print \$2}' | sed -n '1!p'", returnStdout:true).trim()
                    def REPLICAS = sh (script: "kubectl get deploy --all-namespaces --sort-by=.metadata.name | awk '{print \$2,\$6}' | sed -n '1!p'", returnStdout:true).trim()
                    inputService = input(
                        message: "Select a service",
                        parameters: [choice(name: 'Service to deploy', choices: "${SERVICES}", description: 'What service you wont deploy?')]
                    )
                    inputDeploy = input(
                        message: "Select a service",
                        parameters: [choice(name: 'Service to deploy', choices: "${DEPLOY}", description: 'What service you wont deploy?')]
                    )
                    inputReplica = input(
                        message: "Select a service",
                        parameters: [choice(name: 'Service to deploy', choices: "${REPLICAS}", description: 'What service you wont deploy?')]
                    )
                }
            }
        }
        stage('My other Stage'){
            steps{
                script{
                    echo "The selected service is: ${inputService}"
                    echo "The selected service is: ${inputDeploy}"
                    echo "The selected service is: ${inputReplica}"
                    inputSet = input(
                        message: "Please give replicaset number",
                        parameters: [string(defaultValue: "1", description: 'Enter replica number?', name: 'Replicaset')]
                    )
                    sh (script: "kubectl scale --current-replicas=${REPLICAS} --replicas='${params.Replicaset}' deployment/${inputDeploy}")
                }
            }
        }
        stage('Stage-Two'){
            steps {
               sh "echo ${inputService} ${inputDeploy} ${inputReplica}"
            }
        }
    }
}
