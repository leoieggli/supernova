def label = "slave-${UUID.randomUUID().toString()}"

podTemplate(label: label, yaml: """
apiVersion: v1
kind: Pod
metadata:
labels:
    container: slave
spec:
  containers:
  - name: kubectl
    image: lachlanevenson/k8s-kubectl:v1.15.3
    command:
    - cat
    tty: true
  - name: git
    image: alpine/git:latest
    command:
    - cat
    tty: true

"""){
    node(label) {
        container('git') {
            stage("Test Container Git") {
                echo "This is container GIT"
            }
        }   
        container('kubectl') {
            stage("Test Container kubectl") {
                echo "This is container KUBECTL"
            }
        }   
    }
}