def label = "slave-${UUID.randomUUID().toString()}"

podTemplate(label: label, inheritFrom: 'acceptance-slave-pod', cloud: 'paas', containers: [
    containerTemplate(name: 'git', image: 'alpine/git:latest', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.15.3', command: 'cat', ttyEnabled: true)
]){
    
    node(label) {
        container('git') {
            stage("Test Container Git") {
                echo "This is container GIT"
                // sh "git version"
            }
        }   
        container('kubectl') {
            stage("Test Container kubectl") {
                echo "This is container KUBECTL"
                // sh "kubectl version"
            }
        }   
        container('jnlp') {
            stage("Test Container OCP"){
                echo "This is a POD template"
                sh "git version"
                sh "oc version"
                sh "go version"
            }
        }
    }
}

slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was successful"