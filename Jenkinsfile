def label = "slave-${UUID.randomUUID().toString()}"

podTemplate(label: label, inheritFrom: 'acceptance-slave-pod', cloud: 'paas', yaml: """
apiVersion: v1
kind: Pod
metadata:
labels:
    container: slave
spec:
  containers:
  - name: brew
    image: quay.io/homebrew/brew
    command:
    - cat
    tty: true
  - name: docker
    image: docker/compose:1.23.2
    command:
    - cat
    tty: true
    volumeMounts:
    - name: docker
      mountPath: /var/run/docker.sock
  volumes:
  - name: docker
    hostPath:
      path: /var/run/docker.sock
      type: Socket
"""){
    
    node(label) {
        container('brew') {
            stage("Test Container BREW") {
                echo "This is container GIT"
                sh "brew --version"
            }
        }   
        container('docker') {
            stage("Test Container DOCKER") {
                echo "This is container DOCKER"
                sh "docker version"
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