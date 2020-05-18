def label = "slave-${UUID.randomUUID().toString()}"

podTemplate(label: label, inheritFrom: 'acceptance-slave-pod', cloud: 'paas', containers: [
    containerTemplate(name: 'git', image: 'alpine/git:latest', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'docker', image: 'docker/compose:1.23.2', command: 'cat', ttyEnabled: true)]){
    
    node(label) {
        container('git') {
            stage("Test Container Git") {
                echo "This is container GIT"
                // sh "git version"
            }
        }   
        container('docker') {
            stage("Test Container kubectl") {
                echo "This is container DOCKER"
                // sh "docker version"
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

// waitForCIMessage checks: [], overrides: [topic: ''], providerName: 'Red Hat UMB', selector: 'CI_TYPE LIKE "errata.%"'

ciBuildTrigger {
    providers {
      providerDataEnvelope {
        providerData {
          activeMQSubscriber {
            name('Red Hat UMB')
            selector('CI_TYPE LIKE "errata.%"')
            // overrides {
            //   def uuid = "4ba46bbc-949b-11e8-b83f-54ee754ea14c"
            //   topic("Consumer.rh-jenkins-ci-plugin.${uuid}.VirtualTopic.eng.ci.redhat-container-image.pipeline.running")
            // }
            // // Message Checks
            // checks {
            //   msgCheck {
            //     field('$.artifact.type')
            //     expectedValue("cvp")
            //   }
            // }
          }
        }
      }
    }
    noSquash(true)
}
