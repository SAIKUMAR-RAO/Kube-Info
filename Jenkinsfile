pipeline {
    agent { label 'kube'}
    stages {
        stage('Stage-One') {
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
        stage('Stage-Two'){
            steps{
                script{
                    echo "The selected service is: ${inputService}"
                    echo "The selected deployment is: ${inputDeploy}"
                    echo "Current Replicas for selected Deploy is: ${inputReplica}"
                    inputreplicanum = input(
                        message: "Please give replicaset number",
                        parameters: [choice(name: 'Replicas', choices: ['1', '2', '3', '4', '5'], description: 'Pick your requirement')]
                    )
                    sh (script: "kubectl scale --current-replicas=${inputReplica} --replicas=${inputreplicanum} deployment/${inputDeploy}")
                }
            }
        }
        stage('Stage-Three'){
            steps {
               echo "Service selected: ${inputService}"
               echo "Deployment selected: ${inputDeploy}"
               echo "Existing replicas for selected deploy: ${inputReplica}"
               echo "Replicas after scaled: ${inputreplicanum}"
            }
        }
    }
}
