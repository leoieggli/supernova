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



        properties([
            parameters([
                string(
                    name: 'appName',
                    defaultValue: '',
                    description: '* Define the application name'
                ),
                choice(
                    name: 'language',
                    //choices: "Select\ngo\njava\nnodejs\npython\nruby",
                    choices: "Select\ngo\njavascript\njava\nnodejs",
                    description: '* Language that will be used as base to develop the application'
                ),           
                [$class: 'DynamicReferenceParameter', 
                    choiceType: 'ET_FORMATTED_HTML', 
                    description: 'Define the application container port', 
                    name: 'appPort', 
                    randomName: 'choice-parameter-5633384460832175', 
                    referencedParameters: 'language', 
                    script: [
                        $class: 'GroovyScript', 
                        script: [
                            classpath: [],
                            sandbox: true,
                            script: """
                                if (language.equals("Select")){
                                    def message = "echo You must select a language!!!"
                                    return message.execute().in.text
                                } else if (language.equals("javascript")){
                                    def message = "echo Nothing to do!!!"
                                    return message.execute().in.text
                                } else {
                                    inputBox = "<input name='value' class='setting-input' type='text'>"
                                    return inputBox
                                }
                            """
                        ],
                        fallbackScript: [
                            classpath: [], 
                            sandbox: true, 
                            script: """
                                def xiru = "echo Error - " + e.toString()
                                return xiru.execute().in.text
                                
                            """
                        ]
                    ]
                ],
                [$class: 'DynamicReferenceParameter', 
                    choiceType: 'ET_FORMATTED_HTML', 
                    description: 'Define the application number of replicas', 
                    name: 'appReplica', 
                    randomName: 'choice-parameter-5633384460832175', 
                    referencedParameters: 'language', 
                    script: [
                        $class: 'GroovyScript', 
                        script: [
                            classpath: [],
                            sandbox: true,
                            script: """
                                if (language.equals("Select")){
                                    def message = "echo You must select a language!!!"
                                    return message.execute().in.text
                                } else if (language.equals("javascript")){
                                    def message = "echo Nothing to do!!!"
                                    return message.execute().in.text
                                } else {
                                    inputBox = "<input name='value' class='setting-input' type='text'>"
                                    return inputBox   
                                }
                            """
                        ],
                        fallbackScript: [
                            classpath: [], 
                            sandbox: true, 
                            script: """
                                def xiru = "echo Error - " + e.toString()
                                return xiru.execute().in.text
                                
                            """
                        ]
                    ]
                ],

                [$class: 'CascadeChoiceParameter', 
                    choiceType: 'PT_SINGLE_SELECT', 
                    description: 'Namespaces that will be used', 
                    name: 'namespaces', 
                    randomName: 'choice-parameter-5633384460832175', 
                    referencedParameters: 'language', 
                    script: [
                        $class: 'GroovyScript', 
                        script: [
                            classpath: [],
                            sandbox: true,
                            script: """
                                if (language.equals("Select")){
                                    return["You must select a language!!!"]
                                } else if (language.equals("javascript")){
                                    return["Nothing to do!!!"]                                                     
                                } else {
                                    def kubecmd1 = "kubectl get namespaces  -o jsonpath={.items[*].metadata.name}"
                                    def whatNamespace = kubecmd1.execute().in.text.split().toList()
                                    def listNamespaces = whatNamespace
                                    return listNamespaces

                                }
                            """
                            
                        ],
                        fallbackScript: [
                            classpath: [], 
                            sandbox: true, 
                            script: """
                                if (namespaces.equals("Select")){
                                    return["Error on sh command!!!"]
                                } else {
                                    def execmd = "kubectl get deploy -o jsonpath={.items[*].metadata.name} -n " + namespaces
                                    def china = "echo Error - " + e.toString()
                                    return[china.in.text]
                                }
                            """
                        ]
                    ]
                ],
                booleanParam(
                    name: 'Checkbox', 
                    defaultValue: true, 
                    description: 'Check if it is necessary to add a new'
                ),           
                choice(
                    name: 'jenkinsfiletemplate',
                    choices: "Select\nabc\ndef",
                    description: '* Pipeline template that can be used - <b><s>Please, be careful with the language selected and the template that must be used</b></s><br><br>* Find the pipeline that fits with the app <b>!!!'
                ),              
                [$class: 'DynamicReferenceParameter', 
                    choiceType: 'ET_FORMATTED_HTML', 
                    description: 'Informations about the application on kubernetes', 
                    name: 'infos', 
                    omitValueField: false, 
                    randomName: 'choice-parameter-5633384460832175', 
                    referencedParameters: 'appName,namespaces,language,appPort,jenkinsfiletemplate,appReplica', 
                    script: [
                        $class: 'GroovyScript', 
                        script: [
                            classpath: [],
                            sandbox: true,
                            script: """
                                if (appName.equals("") | jenkinsfiletemplate.equals("Select")| language.equals("Select")){
                                    return["<b>Nothing to do - YOU MUST DEFINE THE * FIELDS!!!</b>"]
                                } else if (language.equals("javascript")){
                                    def command = "echo Your application name: <b>" + appName + "</b><br> Language selected: <b>" + language + "</b><br> Pipeline template: <b>" + jenkinsfiletemplate + "</b>"
                                    return command.execute().in.text
                                } else {
                                    def command = "echo Your application name: <b>" + appName + "</b><br> Application Port: <b>" + appPort + "</b><br> Number of Replicas: <b>" + appReplica +"</b><br> Namespace: <b>" + namespaces + "</b><br> Language selected: <b>" + language + "</b><br> Pipeline template: <b>" + jenkinsfiletemplate + "</b>"
                                    return command.execute().in.text
                                }
                            """
                        ],
                        fallbackScript: [
                            classpath: [], 
                            sandbox: true, 
                            script: """
                                def xiru = "echo Error - Please review your parameter validation!!!" + e.toString()
                                return xiru.execute().in.text
                                
                            """
                        ]
                    ]
                ]
            ])
        ])
                
        // SCAPE FOR REPOSITORY FIRST COMMIT/UPDATES
        if (params.namespaces == "" | params.namespaces == "Select" | params.namespaces == "You must select a language!!!") {

            echo "Nothing to do!!! See review your parameters, ${env.BUILD_USER_ID} :) "

        } else {

            echo "You can do something!!!"

        }
    }   
}

def notifyBuild(String buildStatus, String channel) {
    if (channel == null) channel = "jenkins-tests";
    buildStatus = buildStatus ?: 'SUCCESS'


    def emoji = buildStatus == 'SUCCESS' ? ':boom:' : ':poop:';
    def emojiGoogle = buildStatus == 'SUCCESS' ? '(y)' : '~@~';
    def deployed = buildStatus == 'SUCCESS' ? 'CREATED' : '';
    try {
        googlechatnotification url: "id:google_chat_id", message: """${emojiGoogle} PROJECT *${params.appName}* ${deployed} !!!
            
            `Please, run the organization scan to enable *${params.appName}* pipeline `
            *Jenkins Job Details:* ${env.BUILD_URL}"""

    } catch (e) {
        echo 'Error trying to send  notification: ' + e.toString()
    }
}