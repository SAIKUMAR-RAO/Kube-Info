pipeline {
    agent { label 'kube'}
    stages {
        stage('My Stage') {
            steps {
                script {
                    def GIT_TAGS = sh (script: "helm ls | awk '{ print \$1 }' | sed -n '1!p'", returnStdout:true).trim()
                    inputResult = input(
                        message: "Select a service",
                        parameters: [choice(name: 'Service to deploy', choices: "${branch1}\n${branch2}\n${branch3}", description: 'What branch you wont deploy?')]
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
            steps{
                 sh "echo ${BRANCHDEPLOY}"
            }
        }
    }
}
