def label = "slave-${UUID.randomUUID().toString()}"

podTemplate(label: label, inheritFrom: 'acceptance-slave-pod', cloud: 'paas', containers: [
    containerTemplate(name: 'git', image: 'alpine/git:latest', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'docker', image: 'docker/compose:1.23.2', command: 'cat', ttyEnabled: true)]){
    
    node(label) {

        // Wait for message and store message content in variable
        // def msgContent = waitForCIMessage \
        //     providerName: 'Red Hat UMB', \
        //     selector: 'CI_TYPE LIKE "errata.%"'
        // echo "msgContent = " + msgContent

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

// triggers:
//   - jms-messaging:
//       selector: 'CI_TYPE LIKE "errata.%"'
//       provider-name: 'Red Hat UMB'

properties([
  pipelineTriggers([
    [
      $class: 'CIBuildTrigger',
      noSquash: false,
      providerData: [
        $class: 'ActiveMQSubscriberProviderData',
        checks: [
          [
            expectedValue: '^foo.*bar$',
            field: '$.msg.tag'
          ]
        ],
        name: 'Red Hat UMB',
        overrides: [
          topic: 'Consumer.rh-jenkins-ci-plugin.da3c13a1-021d-4629-81a1-5f522dc1a9e3.VirtualTopic.qe.ci.>'
        ],
        selector: 'CI_TYPE LIKE "errata.%"',
        timeout: 30
      ]
    ]
  ])
])
// waitForCIMessage checks: [], overrides: [topic: ''], providerName: 'Red Hat UMB', selector: 'CI_TYPE LIKE "errata.%"'

// ciBuildTrigger {
//     providers {
//       providerDataEnvelope {
//         providerData {
//           activeMQSubscriber {
//             name('Red Hat UMB')
//             selector('CI_TYPE LIKE "errata.%"')
//           }
//         }
//       }
//     }
//     noSquash(true)
// }
