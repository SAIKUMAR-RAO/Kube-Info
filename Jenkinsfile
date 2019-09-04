pipeline {
    agent { label 'kube'}
    stages {
        stage('My Stage') {
            steps {
                script {
                    def GIT_TAGS = sh (script: "helm ls | awk '{ print \$1 }' | sed -n '1!p'", returnStdout:true).trim()
                    inputResult = input(
                        message: "Select a service",
                        parameters: [choice(name: "git_tag", choices: "${GIT_TAGS}", description: "Git tag")]
                    )
                }
            }
        }
        stage('My other Stage'){
            steps{
                echo "The selected tag is: ${inputResult}"
            }
        }
        stage('Get File Size'){
            steps{
                script {
                    def FILE_SIZE = sh (script: "ls -lrth /var/lib/jenkins/${inputResult} | awk '{ print \$5 }'", returnStdout:true).trim()
                    echo "The size of the file is: ${FILE_SIZE}"
                }
            }
        }
    }
}
