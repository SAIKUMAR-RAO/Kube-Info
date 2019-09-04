pipeline {
    agent { label 'kube'}
    stages {
        stage('My Stage') {
            steps {
                script {
                    def SERVICES = sh (script: "kubectl get svc --all-namespaces --sort-by=.metadata.name | awk '{print \$2}' | sed -n '1!p'", returnStdout:true).trim()
                    def DEPLOY = sh (script: "kubectl get deploy --all-namespaces --sort-by=.metadata.name | awk '{print \$2}' | sed -n '1!p'", returnStdout:true).trim()
                    inputService = input(
                        message: "Select a service",
                        parameters: [choice(name: 'Service to deploy', choices: "${SERVICES}", description: 'What service you wont deploy?')]
                    )
                    inputDeploy = input(
                        message: "Select a deployment",
                        parameters: [choice(name: 'Service to deploy', choices: "${DEPLOY}", description: 'What service you wont deploy?')]
                    )
                    def REPLICAS = sh (script: "kubectl get deploy ${inputDeploy} | awk '{print \$3}' | sed -n '1!p'", returnStdout:true).trim()
                    inputReplica = input(
                        message: "Current replicas for selected Deployment",
                        parameters: [choice(name: 'Current_replicas', choices: "${REPLICAS}", description: 'Current replicas for selected Deployment')]
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
                    inputreplicanum = input(
                        message: "Please give replicaset number",
                        parameters: [choice(name: 'Replicas', choices: ['1', '2', '3'], description: 'Pick something')]
                    )
                    sh (script: "kubectl scale --current-replicas=${inputReplica} --replicas=${inputreplicanum} deployment/${inputDeploy}")
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
